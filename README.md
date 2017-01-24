# AWS 
My scripts to install softwares on amazon Ec2 


git 

```

git config --global user.email "you@example.com"
git config --global user.name "Your Name"
  
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
