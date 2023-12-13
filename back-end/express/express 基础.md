# express 基础

- 初始化项目
    ```bash
    # entry point 使用 app.js
    npm init
    # 安装依赖
    npm install express
    ```

- `app.js` 代码
    ```javascript
    const express = require('express')
    const app = express()
    const port = 3000

    app.get('/', (req, res) => {
      res.send('Hello World!')
    })

    app.listen(port, () => {
      console.log(`Example app listening on port ${port}`)
    })
    ```

- 运行
    ```bash
    npm init
    ```
