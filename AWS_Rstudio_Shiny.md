# Configuring R-studio and Shiny

## Rstudio

```
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list'

gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9

sudo apt-get -y update 
sudo apt-get -y upgrade
sudo apt-get -y install build-essential

#prompt, enter 'Yes' for the codes below
sudo apt-get install r-base 

sudo apt-get install gdebi-core 
wget https://download2.rstudio.org/rstudio-server-1.0.44-amd64.deb
sudo gdebi rstudio-server-1.0.44-amd64.deb
```
#### Configure Port of R-Studio 

```
cd /etc/rstudio

sudo bash -c 'echo "www-port=8080" >> rserver.conf'

```

```
#restart rstudio
sudo restart rstudio-server

```

#### Managing users
```
sudo adduser lowyix
sudo passwd lowyix #to change password 

```


## Shiny 

```

cd
sudo apt-get update
sudo apt-get -y install r-base
sudo su - -c "R -e \"install.packages('shiny', repos='http://cran.rstudio.com/')\""

wget https://download3.rstudio.org/ubuntu-12.04/x86_64/shiny-server-1.5.1.834-amd64.deb

sudo gdebi shiny-server-1.5.1.834-amd64.deb

```

#### Configure port of shiny server

```
cd 
cd /etc/shiny-server
sudo nano shiny-server
```
edit the line `  listen 3838;` to whatever port you require.

```
sudo -s
restart shiny-server
exit

```

#### Start stop shiny

```

sudo start shiny-server
sudo stop shiny-server
```

### Notes 

if you encountered an error `sudo: unable to resolve host ip-54-40-254-81`,
try this: 


```

cd 
cd /etc/
sudo echo "127.0.0.1 ip-xx-xx-xxx-xx" >> hosts
```