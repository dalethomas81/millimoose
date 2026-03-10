# How to make a public key
### MacOS
make folder where ssl key will be stored  
`mkdir -p ~/Documents/tesla-key`

move to created folder  
`cd ~/Documents/tesla-key`

generate private key (DO NOT SHARE THIS!)  
`openssl ecparam -name prime256v1 -genkey -noout -out tesla_private.pem`

generate public key from private key  
`openssl ec -in tesla_private.pem -pubout -out com.tesla.3p.public-key.pem`

optionally read out public key  
`cat com.tesla.3p.public-key.pem`
