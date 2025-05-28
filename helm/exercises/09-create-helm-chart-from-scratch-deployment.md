# Create helm chart from scratch with Deployment object 

# Step 1: Create sample chart 

```
cd
mkdir -p my-charts
cd my-charts
helm create app 
cd app
```

# Step 2: Cleanup 

```
cd templates
rm -fR tests
rm -fR *.yaml 
```
