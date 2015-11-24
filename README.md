mysql01 Cookbook
================

MySQL 5.6 コミュニティ・エディションをダウンロードしてインストールするクックブックです。
このクックブックでは、LinuxディストリビューションのMySQLを利用せず、https://dev.mysql.com/downloads/mysql/ から MySQL v5.6 を入手します。そして、データ領域を "/data1" に作成します。 このため "/data1" をポータブルストレージやiSCSIストレージに設定すれば、サーバーのスケールアップが容易になります。


システム構成
------------

このクックブックは、次の図の様な構成をつくるものです。/data1のファイルシステムにMySQLのデータ領域を移動している処が特徴です。

![MySQLシステム構成](doc/MySQL_config.png)


このクックブックとiSCSIストレージのクックブックを組み合わせる事で、データ移行不要のサーバーのスケールアップが可能になります。
最初は、仮想サーバーに、iSCSIトレージと本クックブックを適用します。仮想サーバーの2CPU構成で始めたとして、そのCPU使用率がフルになってしまったら、ベアメタルサーバーに、iSCSIストレージのクックブックとMySQLのクックブックを適用します。
この際、スケールアップサーバーへ適用するクックブックで、iSCSIストレージのアトリビュート["iscsi_host"]を"standby"に、MySQLのアトリビュート["mysql"]["node"]を"standby"にします。これにより、iSCSIではセッションの確立までに留め、ストレージにファイルシステムを作成したりマウントしたりしません。また、MySQLではインストールと設定ファイルの配置だけに留め起動しません。

![MySQLスケールアップシナリオ](doc/MySQL_Scale_up_story.png)


前提条件(Requirements)
------------
#### 対応オペレーティングシステム
* CentOS 7.x - Minimal Install (64 bit) 
* CentOS 6.x - Minimal Install (64 bit) 
* Ubuntu Linux 14.04 LTS Trusty Tahr - Minimal Install (64 bit) 


アトリビュート(Attributes)
----------

#### mysql01::default
<table>
  <tr>
    <th>Key</th>
    <th>Type</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>["mysql"]["root_password"]</td>
    <td>文字列</td>
    <td>MySQL root パスワード</td>
    <td>NULL (必須)</td>
  </tr>
  <tr>
    <td>["mysql"]["db_name"]</td>
    <td>文字列</td>
    <td>データベース名</td>
    <td>NULL (必須)</td>
  </tr>
  <tr>
    <td>["mysql"]["user"]["name"]</td>
    <td>文字列</td>
    <td>アプリユーザー名</td>
    <td>NULL (必須)</td>
  </tr>
  <tr>
    <td>["mysql"]["user"]["password"]</td>
    <td>文字列</td>
    <td>アプリユーザーパスワード</td>
    <td>NULL (必須)</td>
  </tr>
  <tr>
    <td>["mysql"]["node"]</td>
    <td>文字列</td>
    <td>service/standbyの択一</td>
    <td>service</td>
  </tr>
  <tr>
    <td>["mysql"]["service"]</td>
    <td>文字列</td>
    <td>enable/disableの択一</td>
    <td>service</td>
  </tr>
</table>



使い方(Usage)
-----
#### mysql01::default
TODO: Write usage instructions for each cookbook.

e.g.
Just include `mysql01` in your node's `run_list`:

```json
{
  "name":"my_node",
  "run_list": [
    "recipe[mysql01]"
  ]
}
```

Contributing
------------
TODO: (optional) If this is a public cookbook, detail the process for contributing. If this is a private cookbook, remove this section.

e.g.
1. Fork the repository on Github
2. Create a named feature branch (like `add_component_x`)
3. Write your change
4. Write tests for your change (if applicable)
5. Run the tests, ensuring they all pass
6. Submit a Pull Request using Github

License and Authors
-------------------
Authors: TODO: List authors
