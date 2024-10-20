# Youtube

## 跳過廣告 (2024/09/25)

### 電腦版

``` js
javascript:
(function () {
  function onplay(e) {
    let $video = e.target;
    /* 判斷現在是不是廣告 */
    let $progress = document.querySelector('.ad-interrupting .ytp-play-progress.ytp-swatch-background-color');
    if ($progress) {
      /* 把廣告設為靜音 */
      $video.muted = true;
      /* 等待 3 秒 (太快跳過廣告會被判斷成外掛) */
      setTimeout(() => {
        /* 將廣告影片時間設定到結束前 1 秒 */
        $video.currentTime = parseInt($video.duration - 1);
      }, 3000);
    }
    else {
      /* 取消禁音 */
      $video.muted = false;
    }
  }
  function skipAd() {
    let t = setInterval(() => {
      /* 取得播放器元件 */
      let $video = document.querySelector('#movie_player video');
      if ($video && !$video.classList.contains('custom-skip-ad-player')) {
        $video.classList.add('custom-skip-ad-player');
        $video.addEventListener('play', onplay);
        onplay({ target: $video });
      }
    }, 1000);
  }
  /* 避免重複執行 */
  let $tag = document.querySelector('.custom-skip-ad');
  if (!$tag) {
    /* 尋找 YT Logo 順便判斷是不是在 YT */
    let $logo = document.querySelector('#logo ytd-logo');
    if ($logo) {
      /* 在左上角加入 Skip Ad 字樣 */
      $tag = document.createElement('span');
      $tag.innerText = 'Skip Ad';
      $tag.classList.add('custom-skip-ad');
      $tag.style.fontSize = '14px';
      $tag.style.position = 'absolute';
      $tag.style.top = '4rem';
      $tag.style.left = '11rem';
      $logo.appendChild($tag);
      /* 開始處理廣告 */
      skipAd();
    }
  }
})();
```

mini
``` js
javascript: (function () { function onplay(e) { let $video = e.target; let $progress = document.querySelector('.ad-interrupting .ytp-play-progress.ytp-swatch-background-color'); if ($progress) { $video.muted = true; setTimeout(() => { $video.currentTime = parseInt($video.duration - 1); }, 3000); } else { $video.muted = false; } } function skipAd() { let t = setInterval(() => { let $video = document.querySelector('#movie_player video'); if ($video && !$video.classList.contains('custom-skip-ad-player')) { $video.classList.add('custom-skip-ad-player'); $video.addEventListener('play', onplay); onplay({ target: $video }); } }, 1000); } let $tag = document.querySelector('.custom-skip-ad'); if (!$tag) { let $logo = document.querySelector('#logo ytd-logo'); if ($logo) { $tag = document.createElement('span'); $tag.innerText = 'Skip Ad'; $tag.classList.add('custom-skip-ad'); $tag.style.fontSize = '14px'; $tag.style.position = 'absolute'; $tag.style.top = '4rem'; $tag.style.left = '11rem'; $logo.appendChild($tag); skipAd(); } } })();
```

### 手機版

``` js
javascript:
(function () {
  function isAd(callback, times) {
    if (document.querySelector('.ytp-ad-persistent-progress-bar')) {
      callback();
    }
    else if (times > 0) {
      setTimeout(() => isAd(callback, times - 1), 5);
    }
  }
  function onplay(e) {
    let $video = e.target;
    /* 取消禁音 */
    $video.muted = false;
    /* 等待廣告元件 */
    isAd(() => {
      /* 把廣告設為靜音 */
      $video.muted = true;
      /* 等待 3 秒 (太快跳過廣告會被判斷成外掛) */
      setTimeout(() => {
        /* 將廣告影片時間設定到結束前 1 秒 */
        $video.currentTime = parseInt($video.duration - 1);
      }, 3000);
    }, 50);
  }
  function skipAd() {
    let t = setInterval(() => {
      /* 取得播放器元件 */
      let $video = document.querySelector('.html5-main-video');
      if ($video && !$video.classList.contains('custom-skip-ad-player')) {
        $video.classList.add('custom-skip-ad-player');
        $video.addEventListener('play', onplay);
        onplay({ target: $video });
      }
    }, 1000);
  }
  /* 避免重複執行 */
  let $tag = document.querySelector('.custom-skip-ad');
  if (!$tag) {
    /* 尋找 YT Logo 順便判斷是不是在 YT */
    let $logo = document.querySelector('c3-icon.mobile-topbar-logo');
    if ($logo) {
      /* 在左上角加入 Skip Ad 字樣 */
      $tag = document.createElement('span');
      $tag.innerText = 'Skip Ad';
      $tag.classList.add('custom-skip-ad');
      $tag.style.fontSize = '14px';
      $tag.style.position = 'absolute';
      $tag.style.top = '1.85rem';
      $tag.style.left = '11rem';
      $logo.appendChild($tag);
      /* 開始處理廣告 */
      skipAd();
    }
  }
})();
```

mini
``` js
javascript: (function () { function isAd(callback, times) { if (document.querySelector('.ytp-ad-persistent-progress-bar')) { callback(); } else if (times > 0) { setTimeout(() => isAd(callback, times - 1), 5); } } function onplay(e) { let $video = e.target; $video.muted = false; isAd(() => { $video.muted = true; setTimeout(() => { $video.currentTime = parseInt($video.duration - 1); }, 3000); }, 50); } function skipAd() { let t = setInterval(() => { let $video = document.querySelector('.html5-main-video'); if ($video && !$video.classList.contains('custom-skip-ad-player')) { $video.classList.add('custom-skip-ad-player'); $video.addEventListener('play', onplay); onplay({ target: $video }); } }, 1000); } let $tag = document.querySelector('.custom-skip-ad'); if (!$tag) { let $logo = document.querySelector('c3-icon.mobile-topbar-logo'); if ($logo) { $tag = document.createElement('span'); $tag.innerText = 'Skip Ad'; $tag.classList.add('custom-skip-ad'); $tag.style.fontSize = '14px'; $tag.style.position = 'absolute'; $tag.style.top = '1.85rem'; $tag.style.left = '11rem'; $logo.appendChild($tag); skipAd(); } } })(); 
```

### 整合版

``` js
javascript:
(function () {
  function onplay(e) {
    let $video = e.target;
    /* 判斷現在是不是廣告 */
    let $progress = document.querySelector('.ad-interrupting .ytp-play-progress.ytp-swatch-background-color');
    if ($progress) {
      /* 把廣告設為靜音 */
      $video.muted = true;
      /* 等待 3 秒 (太快跳過廣告會被判斷成外掛) */
      setTimeout(() => {
        /* 將廣告影片時間設定到結束前 1 秒 */
        $video.currentTime = parseInt($video.duration - 1);
      }, 3000);
    }
    else {
      /* 取消禁音 */
      $video.muted = false;
    }
  }
  function skipAd() {
    let t = setInterval(() => {
      /* 取得播放器元件 */
      let $video = document.querySelector('#movie_player video');
      if ($video && !$video.classList.contains('custom-skip-ad-player')) {
        $video.classList.add('custom-skip-ad-player');
        $video.addEventListener('play', onplay);
        onplay({ target: $video });
      }
    }, 1000);
  }
  
  function isAdM(callback, times) {
    if (document.querySelector('.ytp-ad-persistent-progress-bar')) {
      callback();
    }
    else if (times > 0) {
      setTimeout(() => isAdM(callback, times - 1), 5);
    }
  }
  function onplayM(e) {
    let $video = e.target;
    /* 取消禁音 */
    $video.muted = false;
    /* 等待廣告元件 */
    isAdM(() => {
      /* 把廣告設為靜音 */
      $video.muted = true;
      /* 等待 3 秒 (太快跳過廣告會被判斷成外掛) */
      setTimeout(() => {
        /* 將廣告影片時間設定到結束前 1 秒 */
        $video.currentTime = parseInt($video.duration - 1);
      }, 3000);
    }, 50);
  }
  function skipAdM() {
    let t = setInterval(() => {
      /* 取得播放器元件 */
      let $video = document.querySelector('.html5-main-video');
      if ($video && !$video.classList.contains('custom-skip-ad-player')) {
        $video.classList.add('custom-skip-ad-player');
        $video.addEventListener('play', onplayM);
        onplayM({ target: $video });
      }
    }, 1000);
  }
  /* 避免重複執行 */
  let $tag = document.querySelector('.custom-skip-ad');
  if (!$tag) {
    /* 尋找 YT Logo 順便判斷是不是在 YT */
    let $logo = document.querySelector('#logo ytd-logo');
    let $logoM = document.querySelector('c3-icon.mobile-topbar-logo');
    if ($logo) {
      /* 在左上角加入 Skip Ad 字樣 */
      $tag = document.createElement('span');
      $tag.innerText = 'Skip Ad';
      $tag.classList.add('custom-skip-ad');
      $tag.style.fontSize = '14px';
      $tag.style.position = 'absolute';
      $tag.style.top = '4rem';
      $tag.style.left = '11rem';
      $logo.appendChild($tag);
      /* 開始處理廣告 */
      skipAd();
    }
    else if ($logoM) {
      /* 在左上角加入 Skip Ad 字樣 */
      $tag = document.createElement('span');
      $tag.innerText = 'Skip Ad';
      $tag.classList.add('custom-skip-ad');
      $tag.style.fontSize = '14px';
      $tag.style.position = 'absolute';
      $tag.style.top = '1.85rem';
      $tag.style.left = '11rem';
      $logoM.appendChild($tag);
      /* 開始處理廣告 */
      skipAdM();
    }
  }
})();
```
