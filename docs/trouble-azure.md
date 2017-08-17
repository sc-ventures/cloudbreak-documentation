
## Troubleshooting Cloudbreak on Azure 

### Credential Creation Errors

#### Role already exists

**Symptom**: You specified that you want to create a new role for Cloudbreak credential, but an existing role with the same name already exists in Azure.

<a href="../images/new-role-already-exists.png" target="_blank"><img src="../images/new-role-already-exists.png" width="650" title="Role already exists"></a>

**Solution**: You should either rename the role during credential creation or select the `Reuse existing custom role` option. 

#### Role does not exist

**Symptom**: You specified that you want to reuse an existing role for your Cloudbreak credential, but that particular role does not exist in Azure.

<a href="../images/reuse-role-not-exists.png" target="_blank"><img src="../images/reuse-role-not-exists.png" width="650" title="Role does not exist"></a>

**Solution**: You should either rename the new role during the credential creation to match the existing role's name or select the `Let Cloudbreak create a custom role` option. 

#### Role does not have enough privileges 

**Symptom**: You specified that you want to reuse an  existing role for your Cloudbreak credential, but that particular role does not have the necessary privileges for Cloudbreak cluster management.

<a href="../images/reuse-role-insuff-permissions.png" target="_blank"><img src="../images/reuse-role-insuff-permissions.png" width="650" title="Role has insufficent permissions"></a>

**Solution**: You should either select an existing role with enough privileges or select the `Let Cloudbreak create a custom role` option.
 
The necessary action set for Cloudbreak to be able to manage the clusters includes:
        `"Microsoft.Compute/*",
        "Microsoft.Network/*",
        "Microsoft.Storage/*",
        "Microsoft.Resources/*"`
 
#### Cloud not validate publickey certificate

**Symptom**: The syntax of your SSH public key is incorrect.

<a href="../images/invalid-ssh-key.png" target="_blank"><img src="../images/invalid-ssh-key.png" width="650" title="Role already exists"></a>

**Solution**: You must correct the syntax of your SSH key. For information about the correct syntax, refer to [this](https://tools.ietf.org/html/rfc4716#section-3.6) page.
