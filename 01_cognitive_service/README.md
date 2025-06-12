# Setup an Azure Cognitive Service

```bash

CODESPACES=false

source .env

az login -t $TENANT_ID

# Create resource group
az group create -n $BASE_NAME -l $LOCATION

# If needed, purge previously soft-deleted resource
# az cognitiveservices account purge \
# -n $BASE_NAME-ai \
# -g $BASE_NAME \
# -l $LOCATION 

# Create an Azure Cognitive Services resource
az cognitiveservices account create \
-n $BASE_NAME-ai \
-g $BASE_NAME \
-l $LOCATION \
--kind AIServices \
--sku S0


# Get the speech key and set it into an env
export SPEECH_KEY=$(
  az cognitiveservices account keys list \
    -n $BASE_NAME-ai \
    -g $BASE_NAME \
    --query key1 -o tsv
)

# Setup a Personal Voice project
az rest \
  --method put \
  --uri "https://${LOCATION}.api.cognitive.microsoft.com/customvoice/projects/MyPersonalVoiceProject?api-version=2024-02-01-preview" \
  --headers "Ocp-Apim-Subscription-Key=${SPEECH_KEY}" \
  --body '{
    "description": "My demo personal voice project",
    "kind": "PersonalVoice"
  }'






```
