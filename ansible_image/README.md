# How to use this ansible image

1) Download the files in this folder to you host machine

2) Update the hosts file with the IP addresses and other details of your nodes

3) Invoke command to build the image with name say 'myansible_control'
```sh
$ docker build -t myansible_control .
```

4) Once the image is built, you can spin up the container.
```sh
$ docker run --rm --name myansible myansible_control:latest ansible -m ping local
```
## Sample Output:
<pre><font color="#00FF00"><b>osboxes@osboxes</b></font>:<font color="#5C5CFF"><b>~/ansible_image</b></font>$ docker run --rm --name myansible myansible:latest ansible -m ping local
localhost | SUCCESS =&gt; {
    &quot;changed&quot;: false, 
    &quot;ping&quot;: &quot;pong&quot;
}
<font color="#00FF00"><b>osboxes@osboxes</b></font>:<font color="#5C5CFF"><b>~/ansible_image</b></font>$</pre>

Note: local is the group name of nodes in the hosts file. Update the entry in your command as appropriate 

## Limitation: 
This command will invoke new container for each ansible commands you will run. So, we will append --rm into the docker run command, so the container is deleted as soon as the ansible command is finished.
