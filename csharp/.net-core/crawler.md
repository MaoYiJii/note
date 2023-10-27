# Crawler

### 奇摩新聞

```cs
public class YahooNewsCrawler
{
    public class Notification : INotification
    {
        public string Url { get; set; }
        public string CategoryName { get; set; }
    }

    public class Handler : INotificationHandler<Notification>
    {
        private readonly DatabaseContext dbContext;
        private readonly IMediator mediator;
        public Handler(DatabaseContext dbContext, IMediator mediator)
        {
            this.dbContext = dbContext;
            this.mediator = mediator;
        }

        public async Task Handle(Notification notification, CancellationToken cancellationToken)
        {
            Uri uri = new Uri(notification.Url);
            string html = await mediator.Send(new GetHtml.Request() { Url = notification.Url });
            if (html != null)
            {
                HtmlDocument doc = new HtmlDocument();
                doc.LoadHtml(html);
                HtmlNode root = doc.DocumentNode;
                HtmlNodeCollection contents = root.SelectNodes("//div[@id='YDC-Stream']/ul/li[contains(@class, 'js-stream-content')]");
                if (contents != null && contents.Any())
                {
                    foreach (HtmlNode content in contents)
                    {
                        // 過濾掉贊助廣告
                        if (content.SelectSingleNode("./div[contains(@class, 'Feedback')]") != null)
                        {
                            continue;
                        }
                        // 過濾已經存在資料庫的資料
                        string title = content.SelectSingleNode(".//div[contains(@class, 'Cf')]/div/h3/a")?.InnerText;
                        if (await dbContext.Set<External>().AnyAsync(x => x.Title == title))
                        {
                            continue;
                        }
                        // 加入外部內容
                        External external = new External();
                        external.Title = content.SelectSingleNode(".//div[contains(@class, 'Cf')]/div/h3/a")?.InnerText;
                        external.Description = content.SelectSingleNode(".//div[contains(@class, 'Cf')]/div/p")?.InnerText;
                        external.Url = uri.MakeAbsoluteUri(content.SelectSingleNode(".//div[contains(@class, 'Cf')]/div/h3/a")?.GetAttributeValue("href", null));
                        external.Cover = uri.MakeAbsoluteUri(content.SelectSingleNode(".//div[contains(@class, 'Fl(start)')]//img")?.GetAttributeValue("src", null));
                        external.CreateDate = DateTime.Now;
                        await dbContext.AddAsync(external);
                        await dbContext.SaveChangesAsync();
                        // 加入貼文
                        Post post = new Post();
                        post.Type = PostType.External;
                        post.TypeId = external.Id;
                        post.Visible = true;
                        await dbContext.AddAsync(post);
                        await dbContext.SaveChangesAsync();
                        // 加入分類
                        if (string.IsNullOrWhiteSpace(notification.CategoryName) == false)
                        {
                            int parentId = 0;
                            string[] categoryNames = notification.CategoryName.Split(" -> ");
                            foreach (string categoryName in categoryNames)
                            {
                                var category = await dbContext
                                    .Set<Category>()
                                    .FirstOrDefaultAsync(x => x.Name == categoryName && (x.ParentId ?? 0) == parentId);
                                if (category != null)
                                {
                                    if (await dbContext
                                        .Set<PostCategory>()
                                        .AnyAsync(x => x.CategoryId == category.Id && x.PostId == post.Id) == false)
                                    {
                                        PostCategory postCategory = new PostCategory()
                                        {
                                            CategoryId = category.Id,
                                            PostId = post.Id
                                        };
                                        await dbContext.AddAsync(postCategory);
                                    }
                                    parentId = category.Id;
                                }
                                else
                                {
                                    break;
                                }
                            }
                            await dbContext.SaveChangesAsync();
                        }
                    }
                }
            }
        }
    }
}
```
