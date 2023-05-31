# test
Analise - comando abaixo verifica se o username é dev hub
Vale a pena logar dentro do github todas as orgs logadas
sfdx force:org:list
ver se é problema de compatibilidade com o node

Use o comando "sfdx force:auth:logout" para fazer logout de todas as conexões e, em seguida, faça login novamente.

https://gist.github.com/dcarroll/032965d8164fef51c879536486bf68da

SELECT DurableId, SettingValue FROM OrganizationSettingsDetail WHERE SettingName = 'ScratchOrgManagementPref'" -t -u ${usernameOrAlias} --json




- step verificando se é dev hub

hub=$(sfdx force:data:soql:query -q "SELECT DurableId, SettingValue FROM OrganizationSettingsDetail WHERE SettingName = 'ScratchOrgManagementPref'" -t -u ${usernameOrAlias} --json)

# parse response
size="$(echo ${hub} | jq -r .result.size)"

if [[ $size -eq 0 ]];
then
  echo "${usernameOrAlias} is not a dev hub"
  exit 0
fi

records="$(echo ${hub} | jq -r .result.records)"
value="$(echo ${records} | jq -r .[].SettingValue)"

# evaluate the setting value
if $value === 'true'
then
  echo "${usernameOrAlias} is a dev hub"
else
  echo "${usernameOrAlias} is not a dev hub"
fi

exit 0
