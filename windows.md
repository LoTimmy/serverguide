
```xml
<Value>Windows 7 STARTER</Value><!-- Windows 7 簡易版 -->
<Value>Windows 7 HOMEBASIC</Value><!-- Windows 7 家用入門版 -->
<Value>Windows 7 HOMEPREMIUM</Value><!-- Windows 7 家用進階版 -->
<Value>Windows 7 PROFESSIONAL</Value><!-- Windows 7 專業版 -->								
<Value>Windows 7 ULTIMATE</Value><!-- Windows 7 旗艦版 -->
```

**Autounattend.xml** (x86)
```xml
<?xml version="1.0" encoding="utf-8" ?>
<unattend
    xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <component name="Microsoft-Windows-Setup" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS"
            xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <ImageInstall>
                <OSImage>
                    <InstallFrom>
                        <MetaData wcm:action="add">
                            <Key>/IMAGE/INDEX</Key>
                            <Value>Windows 7 PROFESSIONAL</Value>                          
                        </MetaData>
                    </InstallFrom>
                </OSImage>
            </ImageInstall>
            <UserData>
                <AcceptEula>true</AcceptEula>
                <!-- 指定是否要在 Windows 安裝程式執行期間接受 Microsoft 軟體授權合約。 -->
            </UserData>
        </component>
        <component name="Microsoft-Windows-International-Core-WinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS"
            xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <SetupUILanguage>
                <UILanguage>zh-tw</UILanguage>
            </SetupUILanguage>
            <InputLocale>zh-tw</InputLocale>
            <!-- 指定 Windows 安裝的預設輸入法地區設定。 -->
            <UILanguage>zh-tw</UILanguage>
            <!-- 指定 Windows 安裝的預設 UI 語言。 -->
            <UILanguageFallback>zh-tw</UILanguageFallback>
            <UserLocale>zh-tw</UserLocale>
            <!-- 指定 Windows 安裝的預設使用者地區設定。 -->
        </component>
    </settings>
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS"
            xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <OOBE>
                <HideEULAPage>true</HideEULAPage>
                <!--  指定以隱藏 [Microsoft 軟體授權合約] 頁面 -->
                <ProtectYourPC>3</ProtectYourPC>
                <!-- 指定 Windows 安裝的保護等級。 -->
                <SkipMachineOOBE>true</SkipMachineOOBE>
                <SkipUserOOBE>true</SkipUserOOBE>
                <NetworkLocation>Work</NetworkLocation>
                <!-- 指定電腦的位置。 -->
                <HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
                <!-- 隱藏 [無線網路] 選取頁面。 -->
            </OOBE>
            <AutoLogon>
                <Password>
                    <Value></Value>
                    <PlainText>true</PlainText>
                </Password>
                <LogonCount>1</LogonCount>
                <Username>Administrator</Username>
                <Enabled>true</Enabled>
            </AutoLogon>
            <UserAccounts>
                <AdministratorPassword>
                    <Value></Value>
                    <PlainText>true</PlainText>
                </AdministratorPassword>
            </UserAccounts>
        </component>
    </settings>
</unattend>
```

```xml
<?xml version="1.0" encoding="utf-8" ?>
<unattend
    xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <component name="Microsoft-Windows-Setup" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS"
            xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <ImageInstall>
                <OSImage>
                    <InstallFrom>
                        <MetaData wcm:action="add">
                            <Key>/IMAGE/NAME</Key>
                            <!-- <Value>Windows 7 STARTER</Value> -->
                            <!-- Windows 7 簡易版 -->
                            <!-- <Value>Windows 7 HOMEBASIC</Value> -->
                            <!-- Windows 7 家用入門版 -->
                            <!-- <Value>Windows 7 HOMEPREMIUM</Value> -->
                            <!-- Windows 7 家用進階版 -->
                            <Value>Windows 7 PROFESSIONAL</Value>
                            <!-- Windows 7 專業版 -->
                            <!-- <Value>Windows 7 ULTIMATE</Value> -->
                            <!-- Windows 7 旗艦版 -->
                        </MetaData>
                    </InstallFrom>
                </OSImage>
            </ImageInstall>
            <UserData>
                <AcceptEula>true</AcceptEula>
                <!-- 指定是否要在 Windows 安裝程式執行期間接受 Microsoft 軟體授權合約。 -->
            </UserData>
        </component>
        <component name="Microsoft-Windows-International-Core-WinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS"
            xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <SetupUILanguage>
                <UILanguage>zh-tw</UILanguage>
            </SetupUILanguage>
            <InputLocale>zh-tw</InputLocale>
            <!-- 指定 Windows 安裝的預設輸入法地區設定。 -->
            <UILanguage>zh-tw</UILanguage>
            <!-- 指定 Windows 安裝的預設 UI 語言。 -->
            <UILanguageFallback>zh-tw</UILanguageFallback>
            <UserLocale>zh-tw</UserLocale>
            <!-- 指定 Windows 安裝的預設使用者地區設定。 -->
        </component>
    </settings>
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS"
            xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <OOBE>
                <HideEULAPage>true</HideEULAPage>
                <!--  指定以隱藏 [Microsoft 軟體授權合約] 頁面 -->
                <ProtectYourPC>3</ProtectYourPC>
                <!-- 指定 Windows 安裝的保護等級。 -->
                <SkipMachineOOBE>true</SkipMachineOOBE>
                <SkipUserOOBE>true</SkipUserOOBE>
                <NetworkLocation>Work</NetworkLocation>
                <!-- 指定電腦的位置。 -->
                <HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
                <!-- 隱藏 [無線網路] 選取頁面。 -->
            </OOBE>
            <AutoLogon>
                <Password>
                    <Value></Value>
                    <PlainText>true</PlainText>
                </Password>
                <LogonCount>1</LogonCount>
                <Username>Administrator</Username>
                <Enabled>true</Enabled>
            </AutoLogon>
            <UserAccounts>
                <AdministratorPassword>
                    <!-- 設定 Administrator 帳戶密碼。 -->
                    <Value></Value>
                    <PlainText>true</PlainText>
                </AdministratorPassword>
            </UserAccounts>
        </component>
    </settings>
</unattend>

```

#### :books: 參考網站：
- [執行 Windows 安裝程式的方法](https://technet.microsoft.com/zh-tw/library/cc749415(v=ws.10).aspx)
- [iT自救術─x86和x64到底有什麼差異？](http://www.ithome.com.tw/node/56880)
- [Windows 7：常見問題](http://www.dell.com/learn/tw/zh/twbsd1/operating-systems/windows7_smb_faq)
- [逐步解說： Boot Windows PE from CD-ROM](https://technet.microsoft.com/zh-tw/library/cc766385(v=ws.10).aspx)
- [下載 Windows 7 光碟映像 (ISO 檔案)](https://www.microsoft.com/zh-tw/software-download/windows7)
- https://technet.microsoft.com/en-us/library/dn621892.aspx
- https://technet.microsoft.com/zh-tw/library/cc722388(v=ws.10).aspx
- https://technet.microsoft.com/en-us/library/ff715379.aspx
- https://technet.microsoft.com/zh-tw/library/cc749278(v=ws.10).aspx
- https://technet.microsoft.com/zh-tw/library/jj200142(v=ws.11).aspx
- https://technet.microsoft.com/zh-tw/library/dd744547(v=ws.10).aspx
- https://www.microsoft.com/en-ca/software-download/windows7
- https://technet.microsoft.com/zh-tw/library/cc722150(v=ws.10).aspx
- https://technet.microsoft.com/zh-tw/library/ff716385.aspx
- https://winscp.net/eng/docs/guide_windows_openssh_server


---

`winrm`



```
Windows 遠端管理命令列工具

Windows 遠端管理 (WinRM) 是 Microsoft 對於
WS-Management 通訊協定的實作，它可以使用 Web 服務，在
本機與遠端電腦之間提供安全的通訊方式。

使用方式:
  winrm OPERATION RESOURCE_URI [-SWITCH:VALUE [-SWITCH:VALUE] ...]
        [@{KEY=VALUE[;KEY=VALUE]...}]

特定操作的說明:
  winrm g[et] -?        抓取管理資訊。
  winrm s[et] -?        修改管理資訊。
  winrm c[reate] -?     建立管理資源的新執行個體。
  winrm d[elete] -?     移除管理資源的執行個體。
  winrm e[numerate] -?  列出管理資源的所有執行個體。
  winrm i[nvoke] -?     在管理資源上執行方法。
  winrm id[entify] -?   判斷 WS-Management 實作是否正在
                        遠端上電腦執行。
  winrm quickconfig -?  設定此電腦以接受來自其他電腦的
                        WS-Management 要求。
  winrm configSDDL -?   修改 URI 現有的安全描述元。
  winrm helpmsg -?      顯示錯誤碼的錯誤訊息。

相關主題的說明:
  winrm help uris       如何建構資源 URI。
  winrm help aliases    URI 的縮寫。
  winrm help config     設定 WinRM 用戶端與服務設定。
  winrm help certmapping 設定用戶端憑證存取。
  winrm help remoting   如何存取遠端電腦。
  winrm help auth       提供遠端存取的認證。
  winrm help input      提供輸入以建立、設定和叫用。
  winrm help switches   其他參數，例如格式化、選項等。
  winrm help proxy      提供 Proxy 資訊。
```


```
winrm quickconfig -q
winrm quickconfig -transport:https

winrm set winrm/config @{MaxTimeoutms = "1800000"}
winrm set winrm/config/winrs @{MaxMemoryPerShellMB="1024"}
winrm set winrm/config/service @{AllowUnencrypted="true"}
winrm set winrm/config/service/auth @{Basic="true"}
winrm set winrm/config/service/auth @{CredSSP="true"}

winrm set winrm/config/client @{AllowUnencrypted="true"}
winrm set winrm/config/client/auth @{Basic="true"}

net stop winrm
sc config winrm start= auto
net start winrm
```

#### :books: 參考網站：
- https://www.ibm.com/support/knowledgecenter/zh-tw/SS2JEC_7.2.1/com.ibm.license.mgmt.admin.doc/t_configuring_winrm.html
- https://msdn.microsoft.com/en-us/library/aa384372(v=vs.85).aspx
- https://www.ibm.com/support/knowledgecenter/zh-tw/SS4GSP_6.2.3/com.ibm.udeploy.install.doc/topics/agent_install_winrs.html
- https://pubs.vmware.com/orchestrator-plugins/index.jsp#com.vmware.using.powershell.plugin.doc_10/GUID-D4ACA4EF-D018-448A-866A-DECDDA5CC3C1.html
- https://support.symantec.com/en_US/article.TECH97514.html
- https://support.symantec.com/en_US/article.TECH94458.html



---

#### :books: 參考網站：
- https://technet.microsoft.com/zh-tw/library/ff793405.aspx

---


```
reg add "HKCU\Control Panel\International" /f /v sShortTime /t REG_SZ /d "tt hh:mm"
reg add "HKCU\Control Panel\International" /f /v sTimeFormat /t REG_SZ /d "tt hh:mm:ss"

reg add "HKCU\Control Panel\International" /f /v sShortTime /t REG_SZ /d "HH:mm"
reg add "HKCU\Control Panel\International" /f /v sTimeFormat /t REG_SZ /d "HH:mm:ss"
reg add "HKCU\Control Panel\International" /f /v LocaleName /t REG_SZ /d "zh-TW"
```


```
wmic cpu get name
wmic os get caption
```

#### :books: 參考網站：
- https://technet.microsoft.com/en-us/library/cc742162(v=ws.11).aspx





































































































































































































