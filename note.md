# パッケージリストはどこを見たら？
```yaml
    - name: Install nodejs
      yum:
        name: nodejs
        state: present
```


fatal: [localhost]: FAILED! => {"changed": false, "msg": "No package matching 'nodejs' found available, installed or updated", "rc": 126, "results": ["No package matching 'nodejs' found available, installed or updated"]}

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



