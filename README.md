# Unofficial ShareX Installers for Older Windows NT 10.x Builds and NT 6.x
<p align="center"><a href="https://getsharex.com"><img src="https://getsharex.com/img/ShareX_Banner.png" alt="ShareX Banner"/></a></p>
<h3 align="center">Screen capture, file sharing and productivity tool</h3>
<br>
<div align="center">
  <a href="https://github.com/ShareX/ShareX/actions/workflows/build.yml"><img src="https://img.shields.io/github/actions/workflow/status/ShareX/ShareX/build.yml?branch=develop&label=Build&cacheSeconds=3600" alt="GitHub Workflow Status"/></a>
  <a href="./LICENSE.txt"><img src="https://img.shields.io/github/license/ShareX/ShareX?label=License&color=brightgreen&cacheSeconds=3600" alt="License"/></a>
  <a href="https://github.com/ShareX/ShareX/releases/latest"><img src="https://img.shields.io/github/v/release/ShareX/ShareX?label=Release&color=brightgreen&cacheSeconds=3600" alt="Release"/></a>
  <a href="https://getsharex.com/downloads"><img src="https://img.shields.io/github/downloads/ShareX/ShareX/total?label=Downloads&cacheSeconds=3600" alt="Downloads"/></a>
  <a href="https://discord.gg/ShareX"><img src="https://img.shields.io/discord/194170124859736065?label=Discord&cacheSeconds=3600" alt="Discord"/></a>
  <a href="https://twitter.com/intent/follow?screen_name=ShareX"><img src="https://img.shields.io/twitter/follow/ShareX?cacheSeconds=3600" alt="Twitter"/></a>
</div>
<br>
<p align="center"><a href="https://getsharex.com"><img src="https://getsharex.com/img/ShareX_Screenshot.png" alt="ShareX Screenshot"/></a></p>
<p align="center">For further information please check our <a href="https://getsharex.com">website</a></p>

## How to Build
You need two programs.

[InnoUnpacker](https://github.com/jrathlev/InnoUnpacker-Windows-GUI)

[Inno Setup](https://jrsoftware.org/isinfo.php)

Open InnoUnpacker and locate your ShareX Installer Files.

Check `Process embedded files` and click `Extract Files`.

Open Inno Setup Compiler and locate your `install_script.iss`.

Add this code in bottom.
```
[Code]
procedure InitializeWizard;
begin
  if not IsAdmin then
  begin
    WizardForm.DirEdit.Text := ExpandConstant('{userpf}\ShareX.exe');
  end;
end;

function InitializeUninstall(): Boolean;
var
  ErrorCode: Integer;
begin
  if CheckForMutexes('ShareX.exe') then
  begin
    if MsgBox('Uninstall has detected that ShareX.exe is currently running.' + #13#10#13#10 + 'Would you like to close it?', mbError, MB_YESNO) = IDYES then
    begin
      Exec('taskkill.exe', '/f /im ShareX.exe', '', SW_HIDE, ewWaitUntilTerminated, ErrorCode);
    end
    else
    begin
      Result := False;
      Exit;
    end;
  end;

  Result := True;
end;

function CmdLineParamExists(const value: string): Boolean;
var
  i: Integer;
begin
  Result := False;
  for i := 1 to ParamCount do
    if CompareText(ParamStr(i), value) = 0 then
    begin
      Result := True;
      Exit;
    end;
end;

function IsUpdating(): Boolean;
begin
  Result := CmdLineParamExists('/UPDATE');
end;

function IsNoRun(): Boolean;
begin
  Result := CmdLineParamExists('/NORUN');
end;

function IsPuushMode(): Boolean;
begin
  Result := CmdLineParamExists('-puush');
end;

function DesktopIconExists(): Boolean;
begin
  Result := FileExists(ExpandConstant('{userdesktop}\ShareX.lnk'));
end;
```

Now click Play Button and done.
