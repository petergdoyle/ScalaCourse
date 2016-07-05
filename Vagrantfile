
Vagrant.configure(2) do |config|

  config.vm.box = "petergdoyle/CentOS-7-x86_64-Minimal-1503-01"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "80"]
    vb.cpus=2
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: <<-SHELL

  #best to update the os
  yum -y update && yum -y clean
  yum -y install vim htop curl wget tree unzip bash-completion


  eval "yum repolist |grep 'epel/x86_64'" > /dev/null 2>&1
  if [ $? -eq 1 ]; then
    yum -y install epel-release
  else
    echo -e "\e[7;44;96m*epel-release already appears to be installed. skipping."
  fi

  eval 'java -version' > /dev/null 2>&1
  if [ $? -eq 127 ]; then
    mkdir -p /usr/java
    #install java jdk 8 from oracle
    curl -O -L --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
    "http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jdk-8u60-linux-x64.tar.gz" \
      && tar -xvf jdk-8u60-linux-x64.tar.gz -C /usr/java \
      && ln -s /usr/java/jdk1.8.0_60/ /usr/java/default \
      && rm -f jdk-8u60-linux-x64.tar.gz

    alternatives --install "/usr/bin/java" "java" "/usr/java/default/bin/java" 99999
    alternatives --install "/usr/bin/javac" "javac" "/usr/java/default/bin/javac" 99999
    alternatives --install "/usr/bin/javaws" "javaws" "/usr/java/default/bin/javaws" 99999
    alternatives --install "/usr/bin/jvisualvm" "jvisualvm" "/usr/java/default/bin/jvisualvm" 99999

    export JAVA_HOME=/usr/java/default
    cat >/etc/profile.d/java.sh <<-EOF
export JAVA_HOME=$JAVA_HOME
EOF

  else
    echo -e "\e[7;44;96m*jdk-8u60-linux-x64 already appears to be installed. skipping."
  fi


  eval 'scala -version' > /dev/null 2>&1
  if [ $? -eq 127 ]; then
    mkdir -p /usr/scala
    curl -O -L "http://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz" \
      && tar -xvf scala-2.11.8.tgz -C /usr/scala \
      && ln -s /usr/scala/scala-2.11.8/ /usr/scala/default \
      && rm -f scala-2.11.8.tgz

    alternatives --install "/usr/bin/scala" "scala" "/usr/scala/default/bin/scala" 99999
    alternatives --install "/usr/bin/scalac" "scalac" "/usr/scala/default/bin/scalac" 99999
    alternatives --install "/usr/bin/scaladoc" "scaladoc" "/usr/scala/default/bin/scaladoc" 99999
    alternatives --install "/usr/bin/scalap" "scalap" "/usr/scala/default/bin/scalap" 99999

    export SCALA_HOME=/usr/scala/default
    cat >/etc/profile.d/scala.sh <<-EOF
export SCALA_HOME=$SCALA_HOME
EOF

  else
    echo -e "\e[7;44;96m*scala-2.11.8 already appears to be installed. skipping."
  fi


  eval 'sbt help' > /dev/null 2>&1
    if [ $? -eq 127 ]; then
      mkdir -p /usr/sbt
      curl -O -L https://dl.bintray.com/sbt/native-packages/sbt/0.13.11/sbt-0.13.11.tgz \
      && tar -xvf sbt-0.13.11.tgz -C /usr/sbt \
      && mv /usr/sbt/sbt /usr/sbt/sbt-0.13.11 \
      && ln -s /usr/sbt/sbt-0.13.11 /usr/sbt/default \
      && rm -f sbt-0.13.11.tgz

      alternatives --install "/usr/bin/sbt" "sbt" "/usr/sbt/default/bin/sbt" 99999

      #this command will install sbt but it takes a while
      sbt

  else
    echo -e "\e[7;44;96m*sbt-0.13.11 already appears to be installed. skipping."
  fi


  #set hostname
  hostnamectl set-hostname ScalaCourse.vbx

  SHELL
end
