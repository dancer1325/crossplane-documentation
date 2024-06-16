# Goal
* Connect Crossplane -- by [UpBound AWS Provider](https://marketplace.upbound.io/providers/upbound/provider-family-aws/v1.7.0) to -> AWS to
  * create MR
  * build & access custom API 

## Prerequisites
* Kubernetes cluster /  RAM  >2 GB
  * permissions to create pods and secrets in the Kubernetes cluster
* [Helm](https://helm.sh/) v3.2.0+
* AWS account / permissions to create an S3 storage bucket
* AWS [access keys](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-creds)
  * [how to generate them](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-configure-quickstart-creds)?

## Steps
* Follow '../Install, Upgrade & Uninstall'
* `kubectl create secret generic aws-secret -n crossplane-system --from-file=creds=./aws-credentials.txt`
  * create Kubernetes Secret with AWS credentials
  * Problems:
    * Problem1: .txt can NOT interpolate environment variables
      * Solution: Add directly the real values
* `kubectl apply -f AWSCrossplane.yaml`
  * install AWS S3 provider & ProviderConfig & S3 bucket as MR
    * `kubectl get providers`
      * check the provider installed
        * 'upbound-provider-family-aws'   manages authentication to AWS
    * `kubectl get crds`
      * check the new CRDs created
* `kubectl create -f AWSCrossplaneS3Bucket.yaml`
  * Note: `kubectl apply -f AWSCrossplane.yaml` NOT valid, because 'generateName' is used
  * `kubectl get buckets`
    * check it's indeed SYNCED "TRUE" & READY "TRUE"
  * `aws s3 ls`
    * confirm that S3 bucket is created
* `kubectl delete bucket BucketNameFoundInPreviousCommand`
  * delete S3 bucket
* `kubectl apply -f AWSDynamoDb.yaml`
  * `kubectl get providers`
    * check the provider installed and healthy
* Define a custom API
  ```
  apiVersion: database.example.com/v1alpha1
  kind: NoSQL
  metadata:
    name: my-nosql-database
  spec:
    location: "US"
  ```
  * `apiVersion` == `APIGroup/version`
    * NO restrictions, BUT recommended follow [Kubernetes API versioning guidelines](https://kubernetes.io/docs/reference/using-api/#api-versioning)
  * `APIGroup`
    * := logical collection of related APIs
      * == collection of individual kinds
  * `kind`
    * == individual resources
    * restrictions
      * [UpperCamelCase](https://kubernetes.io/docs/contribute/style/style-guide/#use-upper-camel-case-for-api-objects)
  * `spec`
    * most important part of an API
    * allows
      * defining the inputs
        * -> rest are NON configurable
* Install the API -- via -- XRD
  * `kubectl apply -f AWSCompositeResourceDefinition.yaml`
  * `kubectl get xrd`
    * check XRD is installed
  * `kubectl api-resources | grep nosql`
    * check the custom API endpoints
      * XC  -- at namespace scope --
      * XR  -- at cluster scope -- 
* `kubectl apply -f AWSComposition.yaml`
  * Create the composition
  * `kubectl get composition`
    * check the composition
* `kubectl apply -f NOSQL.yaml`
  * create `NOSQL` object
  * `kubectl get nosql`
    * check the resource created -- accessible at -- cluster-scope
  * `kubectl get managed`
    * check MR
  * `aws s3 ls`
    * check another S3 bucket created via another custom API
* Create a Claim, which operates at namespace-scope
  * `kubectl create namespace crossplane-test`
    * create dedicated namespace
  * `kubectl apply -f NOSQLClaim.yaml`
    * create the claim in the previously namespace
  * `kubectl get claim -n crossplane-test`
    * check the claim is created!!
  * `kubectl get composite` & `kubectl get managed`
    * check how the claim -> creates a composite resource -> creates the MR
  * `kubectl delete claim -n crossplane-test my-nosql-database`
    * delete the claim & ALL resources related
      * `kubectl get composite` & `kubectl get managed`