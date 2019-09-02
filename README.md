## GraphQL sample sender

Send all possible graphql requests defined in SDL throw proxy. Helps in security testing graphql applications.

Input is output of `gql-generator`, witch parse SDL.

https://github.com/timqian/gql-generator

```
npm install gql-generator -g
gqlg --schemaFilePath ./example/sampleTypeDef.graphql --destDirPath ./example/output --depthLimit 2 --includeDeprecatedFields
```
`depthLimit` should equals 2

You have to specify  
`files` - list of gpl files from `gqlg` with requests  
`target_uri` - target uri  
`http_proxy` if you want to catch requests in Burp  
`dafault_table` is default values for common types.  

Script sends multiple HTTP requests for one request in file if request takes parameters with type with multiple default values.

For example 
`query1(var1: $var1, var2: $var2){..`

if var1 and var2 have type ID!, than script sends 4 request:
```
var1: "1", var2: "1"
var1: "1", var2: "9ed466cc-c371-11e6-934f-1579baf24309"
var1: "9ed466cc-c371-11e6-934f-1579baf24309", var2: "1"
var1: "9ed466cc-c371-11e6-934f-1579baf24309", var2: "9ed466cc-c371-11e6-934f-1579baf24309"
```

If parameter is nullable script exclude it.
