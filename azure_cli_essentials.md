# Azure cli essential

## login

`az login`  
This will prompt a browsr base login

To check current signned user: `az ad signed-in-user show`  

To be sure to clear your terminal session:
`az logout` this logout the current user.  
But you may have more than one user connected (if you switch from std to privilege account), in this case use: `az account clear` to logout all users.


## change current subscription
az account set --subscription ${subscription_name}
