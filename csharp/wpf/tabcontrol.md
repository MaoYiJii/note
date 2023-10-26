# TabControl

### 通用範本

```xaml
<TabControl Margin="5" TabStripPlacement="Left" FontSize="14" ItemsSource="{Binding Files}">
    <!-- Tab 的頁籤 -->
    <TabControl.Resources>
        <StackPanel x:Key="StackPanel_TabItem_Header" x:Shared="False">
            <TextBlock Text="{Binding FileName, Mode=OneWay}" />
        </StackPanel>
    </TabControl.Resources>
    <TabControl.ItemContainerStyle>
        <Style TargetType="TabItem" BasedOn="{StaticResource LeftTabItemStyle}">
            <Setter Property="Header" Value="{StaticResource StackPanel_TabItem_Header}" />
            <Setter Property="ToolTip" Value="{Binding Path, Mode=OneWay}" />
        </Style>
    </TabControl.ItemContainerStyle>
    <!--Tab 控制項樣板-->
    <TabControl.Template>
        <ControlTemplate TargetType="{x:Type TabControl}">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <ScrollViewer HorizontalScrollBarVisibility="Disabled" VerticalScrollBarVisibility="Auto">
                    <TabPanel IsItemsHost="True" />
                </ScrollViewer>
                <ContentPresenter Grid.Column="1" ContentSource="SelectedContent">
                    <ContentPresenter.ContentTemplate>
                        <DataTemplate>
                            <!-- 內容樣板 -->
                        </DataTemplate>
                    </ContentPresenter.ContentTemplate>
                </ContentPresenter>
            </Grid>
        </ControlTemplate>
    </TabControl.Template>
</TabControl>
```
