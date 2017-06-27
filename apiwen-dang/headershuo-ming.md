# header说明

1. 除了认证`/v2/auth`接口，其余接口都需要在header中加入`x-auth-token: {token}`来进行权限校验
2. 如果请求有body，则需要在header中加入`Content-Type: application/json`
