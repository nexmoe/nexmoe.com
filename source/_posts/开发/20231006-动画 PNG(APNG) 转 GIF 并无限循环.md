今天在网上找了些 PNG 格式的动态表情包~~我是不会告诉你是我是在 LINE 偷的表情包的~~，于是了解到是 APNG 这种格式。由于微信和 QQ 不支持 APNG，所以就把 APNG 转为 GIF 了，在使用 APNG 转换成 GIF 后，发现在微信上只能播放一次，就产生了如何批量修改 GIF 的循环次数的问题。

所以准备简单介绍一下 APNG。并提供了一个在线工具，可以将 APNG 批量转换为 GIF，但是该工具不能实现无限循环。所以分享了一个批量修改 GIF 循环次数的方法，使用了 Node.js 和批处理脚本两种不同的实现方式。方便 Node 开发者和使用 Windows 的普通用户直接批量处理。

## APNG 是什么？

APNG（Animated Portable Network Graphics）是 PNG 的位图动画扩展，可以实现 PNG 格式的动态图片效果。APNG 相比于 GIF 在图片质量和细节表现方面更有优势，而且随着越来越多的浏览器对 APNG 的支持，它有望成为下一代动态图的标准之一。主要有以下区别：

1. 图片质量：GIF 最多支持 256 种颜色，并且不支持 Alpha 透明通道，这导致 GIF 在色彩丰富的图片和含有半透明效果的图片上质量较差。而 APNG 可以支持更高质量的图片，包括更多的颜色和 Alpha 透明通道，使得动画效果更加细腻。

2. 构成原理：APNG 和 GIF 都是由多帧构成的动画，但是 APNG 的构成原理允许超自然向下兼容。APNG 的第一帧是标准的 PNG 图片，即使浏览器不认识 APNG 后面的动画数据，也可以无障碍显示第一帧。而如果浏览器支持 APNG，就可以播放后面的帧，实现动画效果。

3. 浏览器支持：从 Chrome 59 开始，Chrome 浏览器开始支持 APNG，使得大部分浏览器都能显示 APNG 动画。唯独 IE 浏览器不支持 APNG。

更多内容请参考：<https://xtaolink.cn/268.html>

## APNG 批量转 GIF

该工具可以批量将 APNG 转为 GIF，不过不能无限循环。

<https://cdkm.com/cn/png-to-gif>

## 批量修改 GIF 为无限循环

### bat（普通用户请使用该方法）

下面是使用批处理脚本（.bat）来实现相同的功能：

```bat
@echo off
setlocal enabledelayedexpansion

set "directoryPath=C:\path\to\directory"

for /r "%directoryPath%" %%f in (*.gif) do (
    echo Modifying %%~nxf
    call :modifyGif "%%f"
)

exit /b

:modifyGif
set "filePath=%~1"

set /p data=<"%filePath%"
set "index=!data:~0,16!"
set "modifiedData=!data:~0,16!!data:~16,1!!data:~17,1!!data:~19!"

echo.!modifiedData!>"%filePath%"

exit /b
```

请将`C:\path\to\directory`替换为实际的目录路径。将上述代码保存为`.bat`文件，双击运行即可。脚本将遍历指定目录下的所有`.gif`文件，并对其进行修改。

请注意，批处理脚本的功能相对有限，无法直接读取二进制文件。上述脚本通过读取文件的第一行来模拟读取文件内容。在修改文件时，它直接将修改后的数据写入文件，而不进行二进制操作。这种方法可能不适用于所有情况，尤其是处理大型文件时可能会有性能问题。如果需要更复杂的二进制文件处理，请考虑使用其他编程语言或工具来实现。

### Node(Nexmoe 使用的该方法）

![](https://i.dawnlab.me/126036c1db438306d478d218435527cc.png)

```javascript
const fs = require('fs');
const path = require('path');

function unlimitedGifRepetitions(path) {
  const data = fs.readFileSync(path);

  const index = data.indexOf(Buffer.from([0x21, 0xFF, 0x0B]));
  if (index < 0) {
    throw new Error(`Cannot find Gif Application Extension in ${path}`);
  }

  data[index + 16] = 255;
  data[index + 17] = 255;

  return data;
}

function batchModifyGifFilesInDirectory(directoryPath) {
  fs.readdir(directoryPath, (err, files) => {
    if (err) {
      console.error('Error reading directory:', err);
      return;
    }

    files.forEach(file => {
      const filePath = path.join(directoryPath, file);
      const fileExtension = path.extname(file);

      if (fileExtension === '.gif') {
        try {
          const modifiedData = unlimitedGifRepetitions(filePath);
          fs.writeFileSync(filePath, modifiedData);
          console.log(`Modified ${file}`);
        } catch (error) {
          console.error(`Error modifying ${file}:`, error);
        }
      }
    });
  });
}

const directoryPath = './path/to/directory';
batchModifyGifFilesInDirectory(directoryPath);
```

请注意，上述代码使用了 Node.js 的文件系统模块（`fs`）来读取和写入文件。此外，需要将`./path/to/directory`替换为实际的目录路径。在执行该脚本之前，请确保已经安装了 Node.js。

该脚本将批量遍历指定目录下的所有文件，并对后缀名为`.gif`的文件调用`unlimitedGifRepetitions`函数进行修改。修改后的数据将写回原始文件。在控制台输出中，你可以看到每个已修改的文件的信息或任何出现的错误信息。

更多内容参考：<https://www.b612.me/golang/232.html>

## 更好的工具

这个批处理工具可以将多个 APNG 文件批量转换为 GIF 文件，并且可以对转换后的 GIF 文件批量设置为无限循环。

<https://github.com/nexmoe/batch-apng2gif>
