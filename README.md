# pygeoapi-cdms-plugin
CDMSProvider() plugin to allow supported CDMSs to be used with pygeoapi

```
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Currently we need to also install the pygeoapi dev requirements file
# (possibly just for `flask_cors`). The requirements file doesn't exist
# in the installed package so we're piping it to pip using curl.
pip install -r <(curl https://raw.githubusercontent.com/geopython/pygeoapi/master/requirements-dev.txt)

# Create an opencdmsdb database container
opencdms-test-data startdb --containers opencdmsdb
sleep 20

# Override current opencdms.config setting for db port
export CDM_DB_PORT=35432

export PYGEOAPI_CONFIG=$(pwd)/pygeoapi-config.yml
export PYGEOAPI_OPENAPI=$(pwd)/pygeoapi-openapi.yml

opencdms seed-db

# If you won't be running pygeoapi on localhost then change the config appropriately,
# e.g. for GitHub codespaces, change server `url` in pygeoapi-config.yml to $CODESPACE_NAME
# However, you wouldn't want to commit this change back to the repository
sed -i "s|url: http://localhost:5000|url: https://${CODESPACE_NAME}-5000.preview.app.github.dev|g" pygeoapi-config.yml

# Run the server
pygeoapi serve

# To stop the server, press CTRL+C
opencdms clear-db
opencdms-test-data stopdb
