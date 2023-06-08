# 通知
- window.showInformationMessage
- window.showWarningMessage
- window.showErrorMessage


```js
vscode.window.showInformationMessage('我是info信息！');
vscode.window.showErrorMessage('我是错误信息！');
```
自定义按钮带回调的提示
```js
vscode.window.showInformationMessage('是否要打开小茗同学的博客？', '是', '否', '不再提示').then(result => {
	if (result === '是') {
		exec(`open 'https://haoji.me'`);
	} else if (result === '不再提示') {
		// 其它操作
	}
});
```
# 状态栏
```js
vscode.window.setStatusBarMessage('你好，前端艺术家！');
```
setStatusBarMessage()的实现基于 `vscode.window.createStatusBarItem`，如果想对状态栏进行更多定制，可以使用createStatusBarItem。

# 输入框
用于文本输入，执行`const text = await vscode.window.showInputBox({})`
如果用户取消，则返回空。

# 内容选择
用于内容选择，执行`const text = await vscode.window.showQuickPick(['1', '2', '3'])`

多选`const text = await vscode.window.showQuickPick(['1', '2', '3'], { canPickMany: true })`

# 文件选择
文件选择`const files = await vscode.window.showOpenDialog()`

文件多选`const files = await vscode.window.showOpenDialog({ canSelectMany: true })`

# 终端操作
创建输出panel
`const channel = vscode.window.createOutputChannel('HelloWorld')`

创建terminal
`const terminal = vscode.window.createTerminal('HelloWorld')`

terminal发送文本
`terminal.sendText('ls -a')`


# 注册命令
```plain
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  vscode.commands.registerCommand('vscode-extension-demo.helloWorld', () => {
    vscode.window.showInformationMessage('Hello World!');      
  });
}
```

带参数和返回值的命令
```plain
// 定义
vscode.commands.registerCommand('vscode-extension-demo.helloWorld', (name = 'World') => {
  vscode.window.showInformationMessage(`Hello ${name}!`);
  return name;
});

// 调用
const name = await vscode.commands.executeCommand('vscode-extension-demo.helloWorld', 'Demo');
console.log(name); // 'Demo'

```


异步回调函数
```plain
// 定义
vscode.commands.registerCommand('vscode-extension-demo.helloWorld', async () => {
  await new Promise(resolve => setTimeout(resolve, 1000));
  vscode.window.showInformationMessage(`Hello World!`);
});

// 调用
await vscode.commands.executeCommand('vscode-extension-demo.helloWorld');
// 1秒后才弹出消息框并打印
console.log('end');
```

# 自定义树形内容
通过TreeView API可以在VS Code的侧边栏中展示树形结构的内容，其样式与VSCode内置样式保持一致。  
创建一个TreeView需要两步：
1. 在pacakge.json中配置视图相关属性
2. 创建一个TreeDataProvider并将其绑定到视图中


package.json
```json
{
  "activationEvents": [
    // 1. 打开树视图时激活插件（不定义则无法触发 TreeDataProvider 的创建和绑定）
    "onView:demoTreeView"
  ],
  "contributes": {
    "viewsContainers": {
      "activitybar": [
        // 2. 往 ActivityBar 添加一个自定义视图容器
        {
          "id": "demoView",
          "title": "Demo视图",
          "icon": "$(list-tree)"
        }
      ]
    },
    "views": {
      // 3. 往自定义视图容器添加视图
      "demoView": [
        {
          "id": "demoTreeView",
          "name": "Demo树视图"
        }
      ]
    }
  }
}
```

TreeView API
```js

import * as vscode from 'vscode';

export function activate() {
  const treeDataProvider: vscode.TreeDataProvider<string> = {
    getChildren(text) {
      if (text) {
        return [1, 2, 3].map(num => `subitem${num}`);
      }
      return [1, 2, 3, 4].map(num => `item${num}`);
    },
    getTreeItem(label) {
      return {
        label,
        collapsibleState: label === 'item3' ? vscode.TreeItemCollapsibleState.Collapsed : vscode.TreeItemCollapsibleState.None,
      };
    },
  };

  // 可以使用 treeview 实例添加状态监听事件
  const treeview = vscode.window.createTreeView('demoTreeView', {
    treeDataProvider,
  });
}
```

# 自定义webview
```js
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  const panel = vscode.window.createWebviewPanel(
    'helloWorld',
    'Hello World',
    vscode.ViewColumn.One,
  );
   
  panel.webview.html = `
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Hello World</title>
    </head>
    <body style="padding: 16px;">
      <div>Hello World!</div>
    </body>
    </html>
  `;
}
```

# 插件和webview之间双向通信
例如markdown的渲染就是一个webview，这个webview需要响应editor中内容的更新
```js
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  const panel = vscode.window.createWebviewPanel(
    'helloWorld',
    'Hello World',
    vscode.ViewColumn.One,
    // 1. 首先需要开启 webview 的脚本能力
    { enableScripts: true },
  );
  
  // 2. 插件侧添加消息订阅函数
  panel.webview.onDidReceiveMessage(message => {
    console.log(`receive message from ${message}`);
    // 向 webview 发送消息
    panel.webview.postMessage('extension');
  });
   
  panel.webview.html = `
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Hello World</title>
    </head>
    <body style="padding: 16px;">
      <div>Hello World!</div>
      <!-- 3. webview 侧添加消息订阅函数 -->
      <script>
        (function() {
          const vscode = acquireVsCodeApi();
          // 向插件侧发送消息
          vscode.postMessage('webview');

          window.addEventListener('message', e => {
            console.log('%c[Webview Host]', 'color:blue;', \`\nreceive message from \${e.data}\`);
          });
        })();
      </script>
    </body>
    </html>
  `;
}
```


# 在侧边栏中显示webview
package.json
```json
{
  "contributes": {
    "views": {
      "demoView": [
        {
          // 需要额外指定该属性
          "type": "webview",
          "id": "demoWebviewView",
          "name": "Demo Webview视图"
        }
      ]
    }
  }
}
```

创建WebViewProvider
```js
import * as vscode from 'vscode';

export function activate() {
  class DemoWebviewViewProvider implements vscode.WebviewViewProvider {
    public resolveWebviewView(webviewView: vscode.WebviewView) {
      webviewView.webview.options = {
        enableScripts: true,
      };

      webviewView.webview.html = `
        <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8">
          <title>Hello World Webview View</title>
        </head>
        <body style="padding: 16px;">
          <div>Hello World Webview View!</div>
        </body>
        </html>
      `;
    }
  }

  vscode.window.registerWebviewViewProvider('demoWebviewView', new DemoWebviewViewProvider());
}
```


# 修改当前激活的编辑器的内容
```js
vscode.window.activeTextEditor.edit(editBuilder => {
	// 从开始到结束，全量替换
	const end = new vscode.Position(vscode.window.activeTextEditor.document.lineCount + 1, 0);
	const text = '新替换的内容';
	editBuilder.replace(new vscode.Range(new vscode.Position(0, 0), end), text);
});
```

# 打开文件并选中某段文字
```js
const path = '/Users/somefile.txt';
const options = {
	// 选中第3行第9列到第3行第17列
	selection: new vscode.Range(new vscode.Position(2, 8), new vscode.Position(2, 16));
	// 是否预览，默认true，预览的意思是下次再打开文件是否会替换当前文件
	preview: false,
	// 显示在第二个编辑器
	viewColumn: vscode.ViewColumn.Two
};
vscode.window.showTextDocument(vscode.Uri.file(path), options);

```