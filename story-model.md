
# Model Story



Let's say we have a brand new model for vehicle detection. Within that model, we may have a few variants:
- vehicle-640p-fp32
- vehicle-320p-int8

They start at version zero (`v0`) and overtime we may add more variants, and each variant can be re-trained creating new versions for them.

These are the events to be simulated:
- vehicle-640p-fp32 is trained, stored and registered. This is v0.
- vehicle-640p-fp32@v0 is downloaded from latest
- vehicle-640p-fp32@v0 is promoted to dev, then to prod.
- vehicle-640p-fp32@v0 is downloaded from prod stage
- vehicle-640p-fp32 is trained again, stored and registered. This is v1.
- vehicle-640p-fp32@v1 is downloaded from latest
- vehicle-640p-fp32@v1 is promoted to dev, then to prod.
- vehicle-640p-fp32@v1 is downloaded from prod stage
- vehicle-320p-int8 is trained, stored and registered. This is v0.
- vehicle-320p-int8@v0 is downloaded from latest
- vehicle-320p-int8@v0 is promoted to dev, then to prod.
- vehicle-320p-int8@v0 is downloaded from prod stage
- vehicle-640p-fp32 is now deprecated (all versions)

Let's control their storage and versioning with DVC + GTO.


```
mkdir -p data/models
echo "v0" > data/models/vehicle-640p-fp32.txt.pt

export REPO=https://github.com/gustavovaliati/data-registry
gto register vehicle-640p-fp32
git push origin vehicle-640p-fp32@v0.0.1
gto assign vehicle-640p-fp32 --stage dev
git push origin vehicle-640p-fp32#dev#1

```



## Config

```
dvc init
dvc remote add myremote .myremote/dvcstore
```