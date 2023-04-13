# ssh-agent
GitHub action to load a key into in memory using a temp disk.

It can be used as an attempt to keep the private ssh key from being written to disk.

## Usage

> :warning: Beware this action only works with passphrase-less key.

```yaml
    - name: Load ssh key in agent
      id: agent
      uses: LeastAuthority/ssh-agent-action@v1
      with:
        private_key: ${{ secrets.SSH_KEY }}
```

Alternatively, assuming the private key file is available on the runner (e.g.: already in a tmpfs), the `private_key` input can be the path to that file.

```yaml
    - name: Load ssh key in agent
      id: agent
      uses: LeastAuthority/ssh-agent-action@v1
      with:
        private_key: /path/to/private_key
```

The action set a variable in the environment which can be used to find the socket to interact with the agent.
By default, the name of the variable is `SSH_AUTH_SOCK` and the path to the socket is `S.agent.ssh`.
Those can be changed respectively with the `auth_sock_name` and `auth_sock_path` inputs.

Without changing anything, most ssh client should now be able to use the key.

```yaml
   - name: Test ssh connection
     run: |
       ssh -a -x -o StrictHostKeyChecking=no alice@server.example.com whoami
```
