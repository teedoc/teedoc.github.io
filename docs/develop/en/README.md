Develop teedoc
===========


## Get the source code

```
git clone https://github.com/Neutree/teedoc
```

## Installation Environment

```
sudo apt install python3 python3-pip
cd teedoc
pip3 install -r requirements.txt
```



## Run the source code

* Install plugin

```
python3 teedoc/teedoc_main.py -p examples/teedoc_site install
```

* Run

```
python3 teedoc/teedoc_main.py -p examples/teedoc_site build
python3 teedoc/teedoc_main.py -p examples/teedoc_site serve
```

* Plug-in import issues and update debugging issues

Set the local path of the plugin in `site_config.json`, for example:
```json
"teedoc-plugin-markdown-parser":{
             "from": "../../plugins/teedoc-plugin-markdown-parser"
         }
```
Then at runtime, the package will be imported from this path first (add this path to `sys.path` and then import) instead of the system path, which can ensure that the modification takes effect in time




## Debug

To debug in vscode, you can directly debug the single file `teedoc_debug.py`

## Contribute

You can `fork` a source code in `github`, after modifying the code, submit `PR`(`pull request`)

