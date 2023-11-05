+++
title = 'Add API Gateway Mapping with AWS CDK'
slug = 'cdk-api-mapping'
date = 2023-11-04T19:37:27-05:00
tags = ["cdk", "python"]
draft = false
+++

## The what

I needed to add base path mapping to an existing domain in API Gateway for a REST API, but the
base path needed to be multi-part. CDK offers two main packages for building API Gateway resources.

- aws_apigateway
- aws_apigtewayv2

Looking at the AWS docs, it specifically points out that `aws_apigatewayv2` is meant for HTTP APIs,
and not REST APIs, so I started with the `aws_apigateway` `BasePathMapping` construct. the result was
the discovery of this not supporting multi-part paths.

## The how

After a lot of trial and error, and just some good old curiosity, I figured out I could use the
`aws_apigatewayv2` `CfnApiMapping` construct to achieve what I needed.

## The code

```python
from aws_cdk import aws_apigatewayv2 as apigatewayv2

apigatewayv2.CfnApiMapping(
    self,
    "ApiMapping",
    api_id=rest_api.rest_api_id,
    domain_name="api.domain.com"
    stage="prod",
    api_mapping_key="v1/path"
)
```
