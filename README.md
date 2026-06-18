# password-manager

Cross platform offline gui and tui password manager.
Crypto stuff done with [pycryptodome][1].
GUI done with [pyside6][2].
TUI done with [textualize][3].

> [!WARNING]
> This is a personal project that intends to manage sensitive data. Just because its open source it doesn't mean its safe. Read trough the source code and build from source. I will provide binaries, these binaries will only ever be found in this repo.

# Goal

I was using Bitwarden for some time, but my anxiety came over me and i made my own password manager. Bitwarden might be open source, but its a lot of code to read trough. Even if you read trough all of it, whos to say that they dont run a backdoored fork?

# Features

Its very limited in its nature to keep the codebase as compact as possible in case you want to fully read trough it. However it does have some nice features:
 - Cross Platform (linux, windows, mac)
 - GUI and CLI
 - Generate random passwords
 - what can it store?:
   - name (website, service, etc...)
   - username
   - email
   - password
   - extra info (like a notes field)
   - creation date
   - last edit date
   - uuid (internal putposes only)

# Usage

The simplest way is to just [download the binaries][6]. However downloading and running binaries is always risky, i encourage to clone/fork the repo and build from source:

Prerequisites:
 - python (duh...)
 - [astral uv][4]

<details><summary>Install and run</summary><br/>

  - clone repo: `git clone https://github.com/p1geondove/password-manager`
  - cd into repo: `cd password-manager`
  - create venv and add packages: `uv sync`
  - run the software: `uv run main.py`

</details>

<details><summary>Build binaries from source</summary><br/>
<ol>
  <li>clone repo: `git clone https://github.com/p1geondove/password-manager`</li>
  <li>cd into repo: `cd password-manager`</li>
  <li>create venv and add packages: `uv sync`</li>
  <li>make binary:</li>
  <ul>
    <li>linux: `./build.sh`</li>
    <li>windows:</li>
    <ol>
      <li>[read trough this][5]</li>
      <li>open powershell as admin</li>
      <li>`Saet-ExecutionPolicy unrestricted`</li>
      <li>back to shell with the password-manager</li>
      <li>`./build.ps1`</li>
    </ol>
  </ul>
</ol>
</details>


# Technical details

<details><sumarry>File layout</sumarry><br/>
  <details><summary>General encrypted file</summary><br/>

  Every file has 5 segments that can be split up like this:
  - Bytes 0-32: salt
  - Bytes 32-48: nonce
  - Bytes 48-64: mac tag
  - Byte 64: content type -> 0=bytes 1=string 2=json
  - Bytes 65-: ciphertext

  </details>
  <details><summary>Password manager file</summary><br/>

  A password data containing file is in essence just json. The python type actually used is a `dict[UUID, PWField]`.
  A UUID in this case is just a python builtin uuid.uuid4() like `e18d1093-fcdb-468b-9e5a-6b4a4b9aff22` for example.
  A PWField is a dataclass containting the uuid again as well as username, email, password, extra info, creation date and last edit date.
  The uuids are internally used as UUID types, but get saved as string, in python parsing uuids like this is as simple as using `uuid.UUID(xyz)` while converting to string is done via `str(xyz)`.
  Times are format `%Y-%m-%d %H:%M:%S.%f` which should be normal iso format, ex: `2026-06-18 23:31:29.104373`, to parse the you can use `datetime.fromisoformat(xyz)`, string conversion is done via `str(xyz)`.

  </details>
</details>

[1]: https://pycryptodome.readthedocs.io/en/latest/src/introduction.html
[2]: https://doc.qt.io/qtforpython-6/index.html
[3]: https://www.textualize.io/
[4]: https://docs.astral.sh/uv/getting-started/installation/
[5]: https://superuser.com/questions/106360/how-to-enable-execution-of-powershell-scripts
[6]: https://github.com/p1geondove/password-manager/releases

