# Vagrant ubuntu custom ssh key

- Repo này demo cho bài post [ở đây](https://github.com/fenixpapu/blogs/blob/master/posts/2023/0418-vagrant-custom-ubuntu.md).

-  Tạo `vagrant init`

```
vagrant init ubuntu/focal64
```

- Cài đặt plugin (do có dùng provider nên mình cần cài plugin): 

```
vagrant plugin install vagrant-hosts
```

- Up box thôi :D

```
vagrant up
```

- Remove known host trong trường hợp `vagrant up` và `vagrant destroy` nhiều lần:

```
rm ~/.ssh/known_hosts
```

- Login thôi nào:

```
vagrant ssh # (trong trường hợp dùng vagrant)
sh root@192.168.56.235 # test này để khi mình chạy ansible
```

- Sau khi test xong xuôi thì clean thôi nào:


```
vagrant halt    # to stop
vagrant destroy # to remove
```



