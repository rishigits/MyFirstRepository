# Building Statsd

The instructions provided below specify the steps to build [Statsd](https://www.npmjs.com/package/statsd) version 0.10.2 on Linux on IBM Z for the following distributions:

*   RHEL (8.6, 8.8, 9.0, 9.2)
*   SLES (15 SP4, 15 SP5)
*   Ubuntu (20.04, 22.04, 23.04)

_**General Notes:**_  
* _When following the steps below please use a standard permission user unless otherwise specified_

* _A directory `/<source_root>/` will be referred to in these instructions, this is a temporary writable directory anywhere you'd like to place it_

#### Step 1: Build using script

If you want to build Statsd using manual steps, go to STEP 2.

Use the following commands to build Statsd using the build [script](https://github.com/linux-on-ibm-z/scripts/tree/master/Statsd). Please make sure you have wget installed.

```
wget -q https://raw.githubusercontent.com/linux-on-ibm-z/scripts/master/Statsd/0.10.2/build_statsd.sh

# Build Statsd
bash build_statsd.sh    [Provide -t option for executing build with tests]
```

If the build completes successfully, go to STEP 5. In case of error, check `logs` for more details or go to STEP 2 to follow manual build steps.

#### Step 2: Install dependencies

  ```bash
  export SOURCE_ROOT=/<source_root>/
  ```
    
* RHEL (8.6, 8.8, 9.0, 9.2)
  ```bash
   sudo yum install -y git wget tar unzip hostname make gcc-c++ xz gzip python3 nmap procps
  ```

* SLES (15 SP4, 15 SP5)
  ```bash
    sudo zypper install -y git wget tar unzip hostname make gcc-c++ xz gzip python3 nmap procps
  ```

* Ubuntu (20.04, 22.04, 23.04)
  ```bash
   sudo apt-get update
   sudo apt-get install -y git wget tar unzip hostname python3 g++ make xz-utils
  ```

* Install Nodejs  
  ```bash
  cd $SOURCE_ROOT
  NODE_VERSION=v18.17.1
  wget https://nodejs.org/dist/${NODE_VERSION}/node-${NODE_VERSION}-linux-s390x.tar.xz
  chmod ugo+r node-${NODE_VERSION}-linux-s390x.tar.xz
  sudo tar -C /usr/local -xf node-${NODE_VERSION}-linux-s390x.tar.xz
  export PATH=$PATH:/usr/local/node-${NODE_VERSION}-linux-s390x/bin
  node -v
  ```
  
#### Step 3: Install Statsd 

* Download Statsd source code
  ```bash
  cd $SOURCE_ROOT
  git clone https://github.com/etsy/statsd.git
  cd statsd
  git checkout v0.10.2
  ```

* Install required npm dependencies
  ```bash
  npm install
  ```		
_**Note:**_ User can run `npm audit fix` command to automatically install compatible updates to vulnerable dependencies.

#### Step 4: Testing (Optional)

* Run test cases
  ```bash
  cd $SOURCE_ROOT/statsd/
  ./run_tests.js
  ```

#### Step 5: Run Statsd daemon
  ```bash
     cd $SOURCE_ROOT/statsd
     node stats.js /path/to/config
  ```
  
#### Step 6: Usage
  ```bash
     Make sure that you have netcat installed before executing the below command
     The basic line protocol expects metrics to be sent in the format:
     <metricname>:<value>|<type>
     e.g. echo "foo:1|c" | nc -u -w2 127.0.0.1 8125
  ```
	
  
#### References: 
* https://github.com/etsy/statsd
