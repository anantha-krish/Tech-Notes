AWS deployment

# Steps
## Bucket Setup
- Search S3 and got S3 menu
- Create a Bucket (unique name)
- Goto Properties> Enable static web hosting
- Goto Permissions> Disable public access Blocking
-![1a90066a089eca20ad37bde461078082.png](../_resources/019c084b12d04821acad3b6f9c0a4c9c.png)
- Edit Bucekt policy using Policy Generator
![d0327ceb6e8bf1657a8c3a1eaa3ad258.png](../_resources/7bbbde6c11ce40d19dde2aa97253b514.png)
- Copy the generated JSON & paste in the page ,save it.
![087d98a5ba54c4b3d275edf222dc2aed.png](../_resources/156bb7a40f644fdcafa2df17c4a89b22.png)

## Amazon CDN Setup
- Search for cloudfront
- Click on create distribution
- Select web distribution
- Select the domain Name
![964421016055254a69e0fd2f0c1fb36c.png](../_resources/24cca655a7a44f2f9eac25876a92c1dd.png)
- Redirrect HTTP to HTTPS in cache behaviour
![cd8041bcd6ff9d1bed765efcb59a43f0.png](../_resources/4bcb34fec47f4af7a284d329af4b73ce.png)
- Save the changes & wait the CDN to deploy state
- Goto General tab> Edit  ... set Default Root object as /container/latest/index.html
- Goto Error pages tab
![0a90983760d062b5b4459afc6aa5d0ab.png](../_resources/e1d19f290f7d42cdb2cb1e58b2082dd9.png)
- Create IAM user for access keys with following settings
![2542da7b39e1e058111a7589d26b928e.png](../_resources/fcf91062701d4a208d8b55eb9630383b.png)
after pressing create
![ab05f37f803de0c97374b1dbb41e2593.png](../_resources/e0b00d8150254e7188cc954bb0f6ff1a.png)

## Github Secrets Setup
- Goto Settings> Secrets & add 3 secrets
![b0181c7e82acc4b1fe440aa86a273fd5.png](../_resources/6ea93088025f41549b02351a64f40bd4.png)
```yaml
      # steps for uploading dist folder to S3
      - uses: chrislennon/action-aws-cli@v1.1
       ## added newly
	   env:
          ACTIONS_ALLOW_UNSECURE_COMMANDS: 'true'
      - run: aws s3 sync dist s3://${{ secrets.AWS_S3_BUCKET_NAME }}/container/latest
        env:
		  ## added newly
          ACTIONS_ALLOW_UNSECURE_COMMANDS: 'true'
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID}}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY}}
```
after code commit you can see workflow triggered

![0d1177456b40a35c670a7961eb9083c7.png](../_resources/485628849017437492a1083f89ea9970.png)

# Deployment error fix

you may face this error 
as the html is trying  to acess main.js
but our main.js is in container/latest/main.js
![3f769efbd1a8276676b394887f651bfb.png](../_resources/0ae5cde663cf49e78bcf01266fbf7644.png)

this can be fixed by adding public path of webpack.config
```js
   mode:'production',
    output :{
        // contenthash resolves caching issue
      filename: '[name].[contenthash].js',
      //its required while accessing objects from s3
      publicPath: '/container/latest/'
    },

```
commit the change & push, it will be resolved