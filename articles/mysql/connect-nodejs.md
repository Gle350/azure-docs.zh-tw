---
title: 快速入門：使用 Node.js 連線 - 適用於 MySQL 的 Azure 資料庫
description: 本快速入門提供數個 Node.js 程式碼範例，供您用來在從 Azure Database for MySQL 連線和查詢資料。
author: savjani
ms.author: pariks
ms.service: mysql
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-js
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 12/11/2020
ms.openlocfilehash: 6a9134e13e3145daea1eed81c4aa8795a0a49950
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97588228"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-in-azure-database-for-mysql"></a>快速入門：使用 Node.js 來連線及查詢適用於 MySQL 的 Azure 資料庫中的資料

在本快速入門中，您將使用 Node.js 連線至適用於 MySQL 的 Azure 資料庫。 接著，您可以使用 SQL 陳述式來查詢、插入、更新和刪除 Mac、Ubuntu Linux 和 Windows 平台中的資料庫所含的資料。 

此主題假設您熟悉如何使用 Node.js 進行開發，但是初次接觸適用於 MySQL 的 Azure 資料庫。

## <a name="prerequisites"></a>Prerequisites

- 具有有效訂用帳戶的 Azure 帳戶。 [免費建立帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
- 適用於 MySQL 的 Azure 資料庫伺服器。 [使用 Azure 入口網站建立適用於 MySQL 的 Azure 資料庫伺服器](quickstart-create-mysql-server-database-using-azure-portal.md)或[使用 Azure CLI 建立適用於 MySQL 的 Azure 資料庫伺服器](quickstart-create-mysql-server-database-using-azure-cli.md)。

> [!IMPORTANT] 
> 確保您用於連線的 IP 位址已使用 [Azure 入口網站](./howto-manage-firewall-using-portal.md)或 [Azure CLI](./howto-manage-firewall-using-cli.md) 新增伺服器的防火牆規則

## <a name="install-nodejs-and-the-mysql-connector"></a>安裝 Node.js 和 MySQL 連接器

根據您的平台，依照適當小節中的指示安裝 [Node.js](https://nodejs.org)。 使用 npm 將 [mysql](https://www.npmjs.com/package/mysql) 套件及其相依性安裝到您的專案資料夾中。

### <a name="windows"></a>Windows

1. 造訪 [Node.js 下載頁面](https://nodejs.org/en/download/) \(英文\)，然後選取您需要的 Windows 安裝程式選項。
2. 建立本機專案資料夾，例如 `nodejsmysql`。 
3. 開啟命令提示字元，然後將目錄變更為專案資料夾，例如 `cd c:\nodejsmysql\`
4. 執行 NPM 工具將 mysql 程式庫安裝至專案資料夾中。

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql
   "C:\Program Files\nodejs\npm" list
   ```

5. 檢查`npm list` 輸出文字，以確認安裝。 版本號碼可能會隨著新的修補檔案發行而有所不同。

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. 執行下列命令，以安裝 **Node.js** 和適用於 Node.js 的 **npm** 套件管理員。

   ```bash
    # Using Ubuntu
    curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
    sudo apt-get install -y nodejs

    # Using Debian, as root
    curl -sL https://deb.nodesource.com/setup_14.x | bash -
    apt-get install -y nodejs
   ```

2. 執行下列命令來建立專案資料夾 `mysqlnodejs`，然後將 mysql 封裝安裝到該資料夾。

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql
   npm list
   ```
3. 檢查 npm 清單輸出文字，以確認安裝。 版本號碼可能會隨著新的修補檔案發行而有所不同。

### <a name="macos"></a>macOS

1. 瀏覽 [Node.js 下載頁面](https://nodejs.org/en/download/)，然後選取您的 macOS 安裝程式。

2. 執行下列命令來建立專案資料夾 `mysqlnodejs`，然後將 mysql 封裝安裝到該資料夾。

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql
   npm list
   ```

3. 檢查`npm list` 輸出文字，以確認安裝。 版本號碼可能會隨著新的修補檔案發行而有所不同。

## <a name="get-connection-information"></a>取得連線資訊

取得連線到 Azure Database for MySQL 所需的連線資訊。 您需要完整的伺服器名稱和登入認證。

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 從 Azure 入口網站的左側功能表中，選取 [所有資源]，然後搜尋您所建立的伺服器 (例如 **mydemoserver**)。
3. 選取伺服器名稱。
4. 從伺服器的 [概觀] 面板，記下 [伺服器名稱] 和 [伺服器管理員登入名稱]。 如果您忘記密碼，您也可以從此面板重設密碼。
 :::image type="content" source="./media/connect-nodejs/server-name-azure-database-mysql.png" alt-text="Azure Database for MySQL 伺服器名稱":::

## <a name="running-the-javascript-code-in-nodejs"></a>在 Node.js 中執行 JavaScript 程式碼

1. 將 JavaScript 程式碼貼入文字檔，然後使用副檔名 .js 來將它儲存到專案資料夾 (例如 C:\nodejsmysql\createtable.js 或 /home/username/nodejsmysql/createtable.js)。
2. 開啟命令提示字元或 Bash 殼層，然後將目錄變更為您的專案資料夾 `cd nodejsmysql`。
3. 若要執行應用程式，請輸入後接檔案名稱的節點命令，例如 `node createtable.js`。
4. 在 Windows 上，如果節點應用程式不在您的環境變數路徑中，則可能需要使用完整路徑來啟動節點應用程式，例如 `"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>連線、建立資料表及插入資料

使用下列程式碼搭配 **CREATE TABLE** 和 **INSERT INTO** SQL 陳述式來連線和載入資料。

[mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來與 MySQL 伺服器互動。 [connect()](https://github.com/mysqljs/mysql#establishing-connections) 函式用來建立伺服器連線。 [query()](https://github.com/mysqljs/mysql#performing-queries) 函式用來對 MySQL 資料庫執行 SQL 查詢。 

將 `host`、`user`、`password` 和 `database` 參數取代為您在建立伺服器和資料庫時所指定的值。

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }
});

function queryDatabase(){
    conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
        if (err) throw err; 
        console.log('Dropped inventory table if existed.');
    })
        conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
        console.log('Created inventory table.');
    })
    conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
        else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
    conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
        console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
    conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
    function (err, results, fields) {
                if (err) throw err;
        console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
    conn.end(function (err) { 
    if (err) throw err;
    else  console.log('Done.') 
    });
};
```

## <a name="read-data"></a>讀取資料

使用下列程式碼搭配 **SELECT** SQL 陳述式來連線和讀取資料。 

[mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來與 MySQL 伺服器互動。 [connect()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來建立伺服器連線。 [query()](https://github.com/mysqljs/mysql#performing-queries) 方法用來執行對 MySQL 資料庫的 SQL 查詢。 結果陣列用來保留查詢的結果。

將 `host`、`user`、`password` 和 `database` 參數取代為您在建立伺服器和資料庫時所指定的值。

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }
    });

function readData(){
    conn.query('SELECT * FROM inventory', 
        function (err, results, fields) {
            if (err) throw err;
            else console.log('Selected ' + results.length + ' row(s).');
            for (i = 0; i < results.length; i++) {
                console.log('Row: ' + JSON.stringify(results[i]));
            }
            console.log('Done.');
        })
    conn.end(
        function (err) { 
            if (err) throw err;
            else  console.log('Closing connection.') 
    });
};
```

## <a name="update-data"></a>更新資料

使用下列程式碼搭配 **UPDATE** SQL 陳述式來連線和讀取資料。 

[mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來與 MySQL 伺服器互動。 [connect()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來建立伺服器連線。 [query()](https://github.com/mysqljs/mysql#performing-queries) 方法用來執行對 MySQL 資料庫的 SQL 查詢。 

將 `host`、`user`、`password` 和 `database` 參數取代為您在建立伺服器和資料庫時所指定的值。

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
           })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>刪除資料

使用下列程式碼搭配 **DELETE** SQL 陳述式來連線和讀取資料。 

[mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來與 MySQL 伺服器互動。 [connect()](https://github.com/mysqljs/mysql#establishing-connections) 方法用來建立伺服器連線。 [query()](https://github.com/mysqljs/mysql#performing-queries) 方法用來執行對 MySQL 資料庫的 SQL 查詢。 

將 `host`、`user`、`password` 和 `database` 參數取代為您在建立伺服器和資料庫時所指定的值。

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
           })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="clean-up-resources"></a>清除資源

若要清除在此快速入門期間使用的所有資源，請使用下列命令刪除資源群組：

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用匯出和匯入來移轉資料庫](./concepts-migrate-import-export.md)
