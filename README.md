# General introduction
This repository video-on-demand-on-aws-cn is used to made the video-on-demand-on-aws sample solutions workable in AWS China Region
The original solution guide: https://docs.aws.amazon.com/solutions/latest/video-on-demand/overview.html
An automated reference implementation leveraging AWS Step Functions and MediaConvert to deploy a scalable fault tolerant Video on demand workflow: https://aws.amazon.com/answers/media-entertainment/video-on-demand-on-aws/

# build package for china region steps
## clone git repository
git clone git@github.com:awslabs/video-on-demand-on-aws.git

## Prepare the CloudFront Aliases (CNAME)
In China region, when create a CloudFront distribution, you must specify a domain name with ICP recordal.

## Run the build-s3-dist.sh script, passing in 2 variables:
```
1. Download the deployment/video-on-demand-on-aws-cn.yaml to TEMP_FOLDER
2. cd video-on-demand-on-aws/deployment/
3. mv video-on-demand-on-aws.yaml video-on-demand-on-aws-global.yaml 
4. cp TEMP_FOLDER/video-on-demand-on-aws-cn.yaml video-on-demand-on-aws.yaml 
5. ./build-s3-dist.sh CODEBUCKET CODEVERSION
CODEBUCKET = the name of the S3 bucket in AWS Ningxia region
CODEVERSION = this will be the subfolder containing the code (video-on-demand-on-aws/codeversion)
For example:
./build-s3-dist.sh vod-mediaconvert-workshop-ray cn-quickstart

6. aws s3 sync dist/ s3://CODEBUCKET/video-on-demand-on-aws/CODEVERSION/ --region cn-northwest-1 --profile cn-northwest-1
For example:
aws s3 sync dist/ s3://vod-mediaconvert-workshop-ray/video-on-demand-on-aws/cn-quickstart/ --region cn-northwest-1 --profile cn-north-1

```

# create the cloudformation stack in China region 
Right now, only AWS Ningxia region launched MediaConvert service, so we need create stack on AWS NingXia Region
1. Use the S3 url contain your video-on-demand-on-aws.template under deployment/dist folder
For example:
https://vod-mediaconvert-workshop-ray.s3.cn-northwest-1.amazonaws.com.cn/video-on-demand-on-aws/cn-quickstart/video-on-demand-on-aws.template
2. Specify your parameters and create the stack
3. Once the stack be executed successfully, record the outputs of stack

Input example: 
CloudFront Alias: www.cf.ray-alb-webapp.top
MediaConvertEndPoint url: https://7sq4jhzpb.mediaconvert.cn-northwest-1.amazonaws.com.cn