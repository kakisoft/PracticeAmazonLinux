# パッケージリストはどこを見たら？
```yaml
    - name: Install nodejs
      yum:
        name: nodejs
        state: present
```

fatal: [localhost]: FAILED! => {"changed": false, "msg": "No package matching 'nodejs' found available, installed or updated", "rc": 126,   "results": ["No package matching 'nodejs' found available, installed or updated"]}

_____________________________________________
# get_url

```yaml

    - name: download bind
      get_url:
        url: https://rpm.nodesource.com/setup_16.x
        dest: .
    - name: installing latest version of node.js
      become: yes
      yum:
        name: nodejs
        state: present
```

fatal: [localhost]: FAILED! => {"changed": false, "msg": "No package matching 'nodejs' found available, installed or updated", "rc": 126, "results": ["No package matching 'nodejs' found available, installed or updated"]}



[WARNING]: Consider using the get_url or uri module rather than running 'curl'.  If you need to use command because get_url or uri is insufficient you can add 'warn: false' to
this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.


_____________________________________________

「 | bash -」を外すとエラーとなる。なぜ？
```yaml
    - name: setup nodejs6.x
      shell: curl -sL https://rpm.nodesource.com/setup_16.x | bash -
    - name: installing latest version of node.js
      yum:
        name: nodejs
        state: present
```


_____________________________________________
# dnf リポジトリ
https://docs.ansible.com/ansible/latest/collections/ansible/builtin/dnf_module.html


