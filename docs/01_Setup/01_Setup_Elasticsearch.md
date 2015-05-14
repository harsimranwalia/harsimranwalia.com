### Setup Elasticsearch on Ubuntu/Debian
<br>

#####**1. Which Java to use - OpenJDK or Oracle JAVA?**
... I personally prefer Oracle JAVA out of experience<br>
... Lets remove openJDK (default) and install Oracle JAVA<br>
... Can be done by either using the following commands or downloading shell script [here](https://app.box.com/s/bin8chszftsz13r1wnu8aion4lrr9dv0)<br>
	sudo apt-get purge openjdk-\*
	wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz
	tar -xvf jdk-7u79-linux-x64.tar.gz
	sudo mkdir -p /usr/lib/jvm
	sudo mv ./jdk1.7.0_79 /usr/lib/jvm/
	sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.7.0_79/bin/java" 1
	sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.7.0_79/bin/javac" 1
	sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.7.0_79/bin/javaws" 1
	sudo chmod a+x /usr/bin/java
	sudo chmod a+x /usr/bin/javac
	sudo chmod a+x /usr/bin/javaws
	sudo chown -R root:root /usr/lib/jvm/jdk1.7.0_79
	sudo update-alternatives --config java

#####**2. Install Elasticsearch**
... The setups assumes we are installing version 1.4.4 (latest stable releaase at time of writing article)<br>
... Can be done by either using the following commands or downloading shell script [here](https://app.box.com/s/0bxyijphgf9wlc1mcucwn7fhas9gkm7w)
	wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.4.4.deb
	sudo dpkg -i elasticsearch-1.4.4.deb
	sudo update-rc.d elasticsearch defaults 95 10

... Runs with the default config on default port 9200 (http)