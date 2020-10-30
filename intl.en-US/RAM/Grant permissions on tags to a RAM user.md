---
keyword: [permission policies for tags bound to Elasticsearch clusters, operation permissions on tags bound to Elasticsearch clusters]
---

# Grant permissions on tags to a RAM user

This topic describes how to grant a RAM user the operation permissions specified in a custom policy on the tags that are bound to Elasticsearch clusters.

**Note:** For information about how to create a custom policy, see [Create a custom policy](/intl.en-US/RAM/Create a custom policy.md). After you create a policy in the RAM console with your Alibaba Cloud account, you must use the RAM console or the RAM SDK to grant the required permissions to a RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

## Authorize a RAM user to create or update tags for specific Elasticsearch clusters

In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted the permissions to create tags for Elasticsearch clusters `es-instance1` and `es-instance2` in the China \(Hangzhou\) region.

```
{
  "Statement": [
    {
      "Action": [
        "elasticsearch:CreateTags"
      ],
      "Effect": "Allow",
      "Resource": [
        "acs:elasticsearch:cn-hangzhou:1234:tags/es-instance1",
        "acs:elasticsearch:cn-hangzhou:1234:tags/es-instance2"
      ]
    }
  ],
  "Version": "1"
}
```

## Authorize a RAM user to delete the tags of specific Elasticsearch clusters

In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted the permissions to delete the tags of Elasticsearch clusters `es-instance1` and `es-instance2` in the China \(Hangzhou\) region.

```
{
  "Statement": [
    {
      "Action": [
        "elasticsearch:RemoveTags"
      ],
      "Effect": "Allow",
      "Resource": [
        "acs:elasticsearch:cn-hangzhou:1234:tags/es-instance1",
        "acs:elasticsearch:cn-hangzhou:1234:tags/es-instance2"
      ]
    }
  ],
  "Version": "1"
}
```

## Authorize a RAM user to query the tags of specific Elasticsearch clusters

In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted the permissions to query the tags of Elasticsearch clusters `es-instance1` and `es-instance2` in the China \(Hangzhou\) region.

```
{
  "Statement": [
    {
      "Action": [
        "elasticsearch:ListTags"
      ],
      "Effect": "Allow",
      "Resource": [
        "acs:elasticsearch:cn-hangzhou:1234:tags/es-instance1",
        "acs:elasticsearch:cn-hangzhou:1234:tags/es-instance2"
      ]
    }
  ],
  "Version": "1"
}
```

## Authorize a RAM user to query the tags of all Elasticsearch clusters

In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted the permissions to query the tags of all Elasticsearch clusters in the China \(Hangzhou\) region.

```
{
  "Statement": [
    {
      "Action": [
        "elasticsearch:ListTags"
      ],
      "Effect": "Allow",
      "Resource": "acs:elasticsearch:cn-hangzhou:1234:tags/*"
    }
  ],
  "Version": "1"
}
```

## Authorize a RAM user to perform operations on Elasticsearch clusters bound with specific tags

In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted the permissions to perform operations only on Elasticsearch clusters bound with the tags `name:liumi`, `env:test`, and `env:pre`.

```
{
  "Statement": [
    {
      "Action": [
        "elasticsearch:ListTags"
      ],
      "Effect": "Allow",
      "Resource": "acs:elasticsearch:cn-hangzhou:1234:tags/*"
    }
  ],
  "Version": "1"
}
```

