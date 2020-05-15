---
layout: post
title: "批量上传youtube视频--upload multiple videos to youtube"
categories: ["node.js"]
description: "批量上传youtube视频--upload multiple videos to youtube"
keywords: node, YouTube
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近在用 YouTube ，想要批量上传视频，在网上没有找到相关的工具，谷歌官方也不提供工具。

于是我自己写了一个脚本：[Youtube Uploader: auto upload multiple youtube videos 批量自动上传youtube视频](https://github.com/zhang0peter/youtube-upload-multi-videos)

这个脚本要解决2个难点：

1.如何登录谷歌账号，谷歌禁止自动登录

2.如何批量自动上传视频- auto upload multiple youtube videos

第一个难点在昨天的文章中解决了：[自动登录谷歌账号失败解决：fail to auto login Google Account in Chrome](https://zhang0peter.com/2020/02/17/chrome-auto/)

解决方法是手动登录Google账号，`puppeteer`安装的chrome默认目录：
`node_modules/puppeteer/.local-chromium/win64-706915/chrome-win/chrome.exe`。

手动打开chrome，然后登录谷歌账号。然后在自动化操作chrome时，需要指定chrome的`userDataDir`目录，不然本地没有cache，就要重新登录。

打开`chrome://version`，查看`Profile Path`:`C:\Users\peter\AppData\Local\Chromium\User Data\Default`, 删除 `Default`， 剩下的路径是 user_Data_Dir: `C:\Users\peter\AppData\Local\Chromium\User Data`。


`Windows Profile Path`:`C:\Users\peter\AppData\Local\Chromium\User Data\Default`      
`Windows user Data Dir`: `C:\Users\peter\AppData\Local\Chromium\User Data`   

`Linux Profile Path`:`/home/vncviewer/.config/chromium/Default`      
`Linux user Data Dir`: `/home/vncviewer/.config/chromium/`   

如果你没有桌面端，或者图像界面，可以把`User Data`目录上传到linux服务器，这样可以直接操作。

第二个难点，我使用`puppeteer`帮我进行浏览器的操作，从而实现了批量上传。

代码和操作指南见仓库:[Youtube Uploader: auto upload multiple youtube videos 批量自动上传youtube视频](https://github.com/zhang0peter/youtube-upload-multi-videos)


初始版本代码如下：

```js
const fs = require("fs");

// load puppeteer
const puppeteer = require('puppeteer');

const window_height = 768;
const window_width = 1366;
const studio_url = "https://studio.youtube.com/";

// directory contains the videos you want to upload
const upload_file_directory = "your video directory";
// change user data directory to your directory
const chrome_user_data_directory = "C:\\Users\\user\\AppData\\Local\\Chromium\\User Data";

const title_prefix = "video title prefix ";
const video_description = "";

const DEFAULT_ARGS = [
    '--disable-background-networking',
    '--enable-features=NetworkService,NetworkServiceInProcess',
    '--disable-background-timer-throttling',
    '--disable-backgrounding-occluded-windows',
    '--disable-breakpad',
    '--disable-client-side-phishing-detection',
    '--disable-component-extensions-with-background-pages',
    '--disable-default-apps',
    '--disable-dev-shm-usage',
    '--disable-extensions',
    // BlinkGenPropertyTrees disabled due to crbug.com/937609
    '--disable-features=TranslateUI,BlinkGenPropertyTrees',
    '--disable-hang-monitor',
    '--disable-ipc-flooding-protection',
    '--disable-popup-blocking',
    '--disable-prompt-on-repost',
    '--disable-renderer-backgrounding',
    '--disable-sync',
    '--force-color-profile=srgb',
    '--metrics-recording-only',
    '--no-first-run',
    '--enable-automation',
    '--password-store=basic',
    '--use-mock-keychain',
];

let files = [];
fs.readdir(upload_file_directory, function (err, temp_files) {
    if (err) {
        console.log('Something went wrong...');
        return console.error(err);
    }
    for (let i = 0; i < temp_files.length; i++) {
        files.push(temp_files[i]);
    }
});

try {
    (async () => {
        const browser = await puppeteer.launch(
            {
                'headless': false,    // have window
                executablePath: null,
                userDataDir: chrome_user_data_directory,
                ignoreDefaultArgs: DEFAULT_ARGS,
                autoClose: false,
                args: ['--lang=en-US,en',
                    `--window-size=${window_width},${window_height}`,
                    '--enable-audio-service-sandbox',
                    '--no-sandbox',
                ],
            }
        );
        let page = await browser.newPage();
        await page.setViewport({'width': window_width, 'height': window_height});
        await page.goto(studio_url, options = {'timeout': 20 * 1000});

        for (let i = 0; i < files.length; i++) {
            const file_name = files[i];
            console.log("now process file:\t" + file_name);

            //click create icon
            await page.click('#create-icon');

            //click upload video
            await page.click('#text-item-0 > ytcp-ve');
            await sleep(500);
            //click select files button and upload file
            const [fileChooser] = await Promise.all([
                page.waitForFileChooser(),
                page.click('#select-files-button > div'), // some button that triggers file selection
            ]);
            await fileChooser.accept([upload_file_directory + file_name]);

            // wait 10 seconds
            await sleep(10_000);

            // title content
            const text_box = await page.$x("//*[@id=\"textbox\"]");
            await text_box[0].type(title_prefix + file_name.replace('.mp4', ''));
            //  await page.type('#textbox', title_prefix + file_name.replace('.mp4',''));
            await sleep(1000);

            // Description content
            await text_box[1].type(video_description);

            await sleep(1000);
            // add video to the second playlists

            // await page.click('#basics > ytcp-video-metadata-playlists > ytcp-text-dropdown-trigger > ytcp-dropdown-trigger > div');
            // await page.click('#items > ytcp-ve:nth-child(3)');
            // await page.click('#dialog > div.action-buttons.style-scope.ytcp-playlist-dialog > ytcp-button.save-button.action-button.style-scope.ytcp-playlist-dialog > div');
            // await sleep(500);

            //click next
            await page.click('#dialog > div > ytcp-animatable.button-area.metadata-fade-in-section.style-scope.ytcp-uploads-dialog > div > div.right-button-area.style-scope.ytcp-uploads-dialog');
            await sleep(1000);
            //click next
            await page.click('#dialog > div > ytcp-animatable.button-area.metadata-fade-in-section.style-scope.ytcp-uploads-dialog > div > div.right-button-area.style-scope.ytcp-uploads-dialog');
            await sleep(1000);
            //click publish now and public
            await page.click('#first-container');
            await page.click('#privacy-radios > paper-radio-button:nth-child(1)');
            await page.click('#done-button');
            await sleep(5000);
            // close
            await page.click('#close-button > div');

            // wait 60 seconds
            await sleep(60 * 1000);
        }
        await browser.close();
    })();

} catch (error) {
    console.log(error);
}


function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
```