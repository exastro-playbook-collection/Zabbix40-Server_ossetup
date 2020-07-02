# Ansible Role: Zabbix40-Server_ossetup

# Trademarks
-----------
* Linuxは、Linus Torvalds氏の米国およびその他の国における登録商標または商標です。
* RedHat、RHEL、CentOSは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* Windows、PowerShellは、Microsoft Corporation の米国およびその他の国における登録商標または商標です。
* Ansibleは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* pythonは、Python Software Foundationの登録商標または商標です。
* Zabbixは、ラトビア共和国にあるZabbix LLCの商標です。
* Oracleは、Oracle International Corporation の米国およびその他の国における登録商標または商標です。
* NECは、日本電気株式会社の登録商標または商標です。
* その他、本ロールのコード、ファイルに記載されている会社名および製品名は、各社の登録商標または商標です。

## Description

本Ansible Roleは統合監視ソフトウェアである"Zabbix Server"の環境設定を行います。
対象バージョンは以下のバージョンです。

- Zabbix 4.0

設定内容は以下の通りです。

- サービス自動起動設定  
サービス自動起動を有効に設定すると、OS起動時にサービスも起動します。
対象となるサービスは次の通りです。
  * mariadb
  * httpd
  * zabbix-server

- サービス再起動設定  
 サービス再起動を有効に設定すると、サービスを再起動します。
 サービスが停止している場合は、サービスを起動します。
 対象となるサービスは次の通りです。
  * mariadb
  * httpd
  * zabbix-server

## Supports

- 管理マシン(Ansibleサーバ)
  * Linux系OS（RHEL/CentOS）
  * Ansible バージョン 2.5以上 (動作確認済み：2.5、2.6)
  * Python バージョン 2.6 または2.7

- 管理対象マシン(構築対象マシン)
  * RHEL7

## Requirements

- 管理マシン(Ansibleサーバ)  
  * 管理対象マシンとroot権限でSSH通信可能であること。

- 管理対象マシン(インストール対象マシン)  
  * SELinuxが無効に設定されていること。
  * firewalldが適切に設定されていること

## Role Variables

Role の変数値について説明します。

### Mandatory variables

実行時には、必ず指定しないといけない変数はありません。

### Optional variables

以下の変数は任意で指定します。

 - サービス自動起動設定
  * ''VAR_Zabbix40SV_chkconfig_mysqld'': DB自動起動設定(on|off|'', default: なし)
    + onを指定すると、自動起動設定を有効にします。
    + offを指定すると、自動起動設定を無効にします。
    + パラメータを設定しないと、現在の設定のまま、変更しません。
  * ''VAR_Zabbix40SV_chkconfig_httpd'': Apache自動起動設定(on|off|'', default: なし)
    + onを指定すると、自動起動設定を有効にします。
    + offを指定すると、自動起動設定を無効にします。
    + パラメータを設定しないと、現在の設定のまま、変更しません。
  * ''VAR_Zabbix40SV_chkconfig_zabbixserver'': Zabbix自動起動設定(on|off|'', default: なし)
    + onを指定すると、自動起動設定を有効にします。
    + offを指定すると、自動起動設定を無効にします。
    + パラメータを設定しないと、現在の設定のまま、変更しません。
 - サービス再起動設定
  * ''VAR_Zabbix40SV_start_mysqld'': DB再起動設定(start|'', default: なし)
    + startを設定すると、サービスを再起動します。サービスが停止している場合は、起動します。
    + パラメータを設定しないと、現在のサービスの起動・停止状態のまま、変更しません。
  * ''VAR_Zabbix40SV_start_httpd'': Apache再起動設定(start|'', default: なし)
    + startを設定すると、サービスを再起動します。サービスが停止している場合は、起動します。
    + パラメータを設定しないと、現在のサービスの起動・停止状態のまま、変更しません。
  * ''VAR_Zabbix40SV_start_zabbixserver'': Zabbix再起動設定(start|'', default: なし)
    + startを設定すると、サービスを再起動します。サービスが停止している場合は、起動します。
    + パラメータを設定しないと、現在のサービスの起動・停止状態のまま、変更しません。

## Dependencies

特にありません。

## Usage

1. 本Roleを用いたPlaybookを作成します。
2. 任意変数を必要に応じて指定します。
3. Playbookを実行します。

## Example Playbook

インストールとすべての設定を行う場合は、提供した以下のRoleを"roles"ディレクトリに配置したうえで、
以下のようなPlaybookを作成してください。

- フォルダ構成
~~~
  - group_vars/
    ・ server1
    ・ server2
  - host_vars/
    ・ host1
    ・ host2
  - roles/
    ・ Zabbix40-Server_install/
    ・ Zabbix40-Server_setup/
    ・ Zabbix40-Server_ossetup/
  - Zabbix40-Server_install.yml
  - Zabbix40-Server_setup.yml
  - Zabbix40-Server_ossetup.yml
  - conf.yml
  - hosts
  - site.yml
~~~

- マスターPlaybook サンプル「Zabbix40-Server_ossetup.yml」
~~~
# Zabbix40-Server_ossetup.yml
 - name: setup OS for Zabbix40_Server
   hosts: all
   gather_facts: yes
   become: yes
   tags:
    - os_setup
   roles:
     - Zabbix40-Server_ossetup
~~~

## Running Playbook

- extra-varsを利用する場合の実行例
> ansible-playbook site.yml -k -i hosts --extra-vars="@conf.yml"

- group\_varsを利用する場合の実行例   
  group\_varsで指定したグループ名がwebserver1の場合
> ansible-playbook site.yml -k -i hosts -l webserver1

- host\_varsを利用する場合の実行例
  host\_varsで指定したホスト名がserver1の場合
> ansible-playbook site.yml -k -i hosts -l server1

- 本roleのみを実行する場合は --tags "ossetup"を付け加える
> ansible-playbook site.yml -k -i hosts --extra-vars="@conf.yml" --tags "ossetup"

# Copyright
Copyright (c) 2018 NEC Corporation

# Author Information
NEC Corporation