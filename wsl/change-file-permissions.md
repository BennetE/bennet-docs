# Change file Permissions in WSL

**Links:**<br/>
https://superuser.com/a/1343737/1789051<br/>
https://github.com/Microsoft/WSL/issues/81#issuecomment-796798258

## Using `wsl.conf`
(from https://github.com/Microsoft/WSL/issues/81#issuecomment-796798258)

1. Open /etc/wsl.conf with your favourite text editor.
2. Paste the following into it:

```toml
[automount]
enabled = true
options = "metadata"
mountFsTab = false
```
3. Close all WSL windows!
4. Open PowerShell.
5. List your WSL distributions:
```powershell
wsl --list
```
6. Pick the one you want to fix. We select "Debian" as an example.
7. Terminate the Debian WSL instance:
```bash
wsl --terminate Debian
```
8. Open a new Debian WSL window. It should load for a couple of seconds, which already indicates that the whole WSL was terminated, before.
9. The fix should be applied now. If it still does not work, repeat all above steps and make absolutely sure, that no WSL windows remain open when applying the fix.
