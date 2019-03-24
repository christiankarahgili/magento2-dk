the /ets/hosts should contain hostname 'magento2u.loc'

Ssh keys should be assigned as env vars RSA and RSA_PUB
unix
export RSA="$(cat ~/.ssh/id_rsa)"
export RSA_PUB="$(cat ~/.ssh/id_rsa.pub)"\n
printenv

Windows


Login / Password
admin
mageU123