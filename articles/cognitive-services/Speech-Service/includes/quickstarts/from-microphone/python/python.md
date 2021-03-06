---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: f26f0ab6da398dcdee307f89b27cca780d08af85
ms.sourcegitcommit: 374d1533ea2f2d9d3f8b6e6a8e65c6a5cd4aea47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85838977"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">创建 Azure 语音资源<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * 请确保你有权访问麦克风，以便进行音频捕获

## <a name="source-code"></a>源代码

创建名为“quickstart.py”的文件并在其中粘贴以下 Python 代码。

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/from-microphone/quickstart.py#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>代码说明

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>生成并运行应用

现在，可以使用语音服务测试语音识别。 

如果在 macOS 上运行此程序，并且它是使用麦克风构建的第一个 Python 应用，则可能需要为该麦克风指定终端访问权限。 打开“系统设置”并选择“安全与隐私” 。 接下来，选择“隐私”并在列表中找到“麦克风” 。 最后，选择“终端”并保存。 

1. **启动应用** - 在命令行中键入：
    ```bash
    python quickstart.py
    ```
2. **开始识别** - 它会提示你说英语短语。 语音将发送到语音服务，转录为文本，并在控制台中呈现。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
