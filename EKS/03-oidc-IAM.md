# commands to configure IAM OIDC provider 

1. If you are using Git Bash / WSL / Linux / macOS
```
export cluster_name=<CLUSTER-NAME>
```

Use "set" Instead of "export"
In Windows cmd, use:
```
set cluster_name=<CLUSTER-NAME>
```

If you're using PowerShell, use:
```
$env:cluster_name = "<CLUSTER-NAME>"
```


2. If you are using Git Bash / WSL / Linux / macOS
```
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)
```

In Windows cmd, use:
```
for /f "delims=/ tokens=5" %i in ('aws eks describe-cluster --name %cluster_name% --query "cluster.identity.oidc.issuer" --output text') do set oidc_id=%i
```

If you're using PowerShell, use:
```
$oidc_id = (aws eks describe-cluster --name $env:cluster_name --query "cluster.identity.oidc.issuer" --output text) -split '/' | Select-Object -Last 1
```

## Check if there is an IAM OIDC provider configured already

3. If you are using Git Bash / WSL / Linux / macOS
```
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4
```

For Windows Command Prompt (cmd)
Use "findstr" instead of "grep" and a "for" loop instead of "cut":
```
for /f "tokens=4 delims=/" %i in ('aws iam list-open-id-connect-providers ^| findstr %oidc_id%') do set oidc_provider=%i
```

If you're using PowerShell, use:
```
$oidc_provider = (aws iam list-open-id-connect-providers | Select-String $oidc_id) -split '/' | Select-Object -Last 1
```

If not, run the below command

4. If you are using Git Bash / WSL / Linux / macOS
```
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve
```

For Windows Command Prompt (cmd)
In cmd, use "%cluster_name%" instead of "$cluster_name":
```
eksctl utils associate-iam-oidc-provider --cluster %cluster_name% --approve
```

If you're using PowerShell, use:
```
eksctl utils associate-iam-oidc-provider --cluster $env:cluster_name --approve
```
