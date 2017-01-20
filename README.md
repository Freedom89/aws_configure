# AWS 
My scripts to install softwares on amazon Ec2 


git 

```
function gitbackup() {
  temp=$(pwd)
  git add .;
  git add *;
  git commit -m $1
  git push;
  cd $temp
}
```
ip address
```
ifconfig | grep 'inet '
```
