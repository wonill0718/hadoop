### Create user “training” with password “training” and add to group wheel for sudo access.
<pre><code>
sudo useradd training -u 2000;
sudo groupadd training -g 2000;
gpasswd -a training wheel
</code></pre>
