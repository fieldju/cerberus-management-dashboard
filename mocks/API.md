FORMAT: 1A
HOST: http://api.nike.com

# Cerberus Management Service

An API to manage secrets and access to said secrets in the Cerberus ecosystem.

# Group Authentication

## User Login [/v2/auth/user]

### Authenticate with Cerberus as a User [GET]

This endpoint will take a Users credentials and proxy the request to Vault to get a Vault token for the user with some extra metadata.

+ Request (application/json)

    + Headers

            Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=

+ Response 200 (application/json)

    + Body

            {
                "status": "mfa_req",
                "client_token": "null",
                "data": {
                    "state_token": "jskljdklaj",
                    "devices": [
                        {
                            "id": "123456",
                            "name": "Google Authenticator"
                        }
                    ]
                }
            }

## App Login [/v1/auth/iam-role]

### Authenticate with Cerberus as an App [POST]

This endpoint will take a Users credentials and proxy the request to Vault to get a Vault token for the user with some extra metadata.

+ Request (application/json)

    + Body

            {
                "iam_role_arn" : "arn:aws:iam::123:role/web"
            }

+ Response 200 (application/json)

    + Body

            {
                "auth": {
                    "client_token": "9a8b5f0e-b41f-3fc7-1c94-3ed4a8057396",
                    "policies": [
                        "web"
                    ],
                    "metadata": {
                        "account_id": "123",
                        "iam_role_name": "web"
                    },
                    "lease_duration": 3600,
                    "renewable": true
                }
            }

## Auth [/v1/auth]

### Logout of Cerberus [DELETE]

This endpoint will take the users `X-Vault-Token` header and proxy to Vault to revoke.

+ Request (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

+ Response 204 (application/json)

# Group Safe Deposit Box

## Get authorized Safe Deposit Box list [/v1/safe-deposit-box]

### Get details for each authorized Safe Deposit Box [GET]

This endpoint will list all the Safe Deposit Box a user is authorized to see.

+ Request (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

+ Response 200 (application/json)

    + Body

            [
                {
                    "id": "fb013540-fb5f-11e5-ba72-e899458df21a",
                    "name": "Web",
                    "path": "app/stage/",
                    "category_id": "f7ff85a0-faaa-11e5-a8a9-7fa3b294cd46"
                },
                {
                     "id": "06f82494-fb60-11e5-ba72-e899458df21a",
                     "name": "OneLogin",
                     "path": "shared/onelogin/",
                     "category_id": "f7ffb890-faaa-11e5-a8a9-7fa3b294cd46"
                }
            ]

### Create a Safe Deposit Box [POST]

This endpoint will create a new Safe Deposit Box

+ Request (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

    + Body

            {
                "name": "Stage",
                "description": "Sensitive configuration properties for the stage micro-service.",
                "category_id": "f7ff85a0-faaa-11e5-a8a9-7fa3b294cd46",
                "owner": "Lst-digital.platform-tools.internal",
                "user_group_permissions": [
                    {
                      "name": "Lst-CDT.CloudPlatformEngine.FTE",
                      "role_id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46"
                    }
                ],
                "iam_role_permissions": [
                    {
                        "account_id": "123",
                        "iam_role_name": "stage",
                        "role_id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46"
                    }
                ]
            }

+ Response 201 (application/json)

    + Headers

            Location: /v1/safe-deposit-box/a7d703da-faac-11e5-a8a9-7fa3b294cd46

    + Body

            {
                "id": "a7d703da-faac-11e5-a8a9-7fa3b294cd46"
            }


## Safe Deposit Box [/v1/safe-deposit-box/{id}]

### Get details for a specific authorized Safe Deposit Box [GET]

This endpoint returns details on a specific Safe Deposit Box.

+ Parameters

    + id (required, string, `a7d703da-faac-11e5-a8a9-7fa3b294cd46`) - The id of the Safe Deposit Box

+ Request (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

+ Response 200 (application/json)

    + body

            {
                "id": "a7d703da-faac-11e5-a8a9-7fa3b294cd46",
                "name": "Stage",
                "description": "Sensitive configuration properties for the stage micro-service.",
                "path": "app/stage/",
                "category_id": "f7ff85a0-faaa-11e5-a8a9-7fa3b294cd46",
                "owner": "Lst-digital.platform-tools.internal",
                "user_group_permissions": [
                    {
                        "id": "3fc6455c-faad-11e5-a8a9-7fa3b294cd46",
                        "name": "Lst-CDT.CloudPlatformEngine.FTE",
                        "role_id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46"
                    }
                ],
                "iam_role_permissions": [
                    {
                        "id": "d05bf72e-faad-11e5-a8a9-7fa3b294cd46",
                        "account_id": "123",
                        "iam_role_name" :"stage",
                        "role_id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46"
                    }
                ]
            }

### Update a specific authorized Safe Deposit Box [PUT]

This endpoint allows a user to update the description, user group, and iam role mappings

+ Request (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

    + Body

            {
                "description": "All configuration properties for the stage micro-service.",
                "owner": "Lst-Squad.Carebears",
                "user_group_permissions": [
                    {
                        "name": "Lst-CDT.CloudPlatformEngine.FTE",
                        "role_id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46"
                    }
                ],
                "iam_role_permissions": [
                    {
                        "account_id": "123",
                        "iam_role_name" :"stage",
                        "role_id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46"
                    }
                ]
            }

+ Response 204

# Group role

## Role List [/v1/role]

### Retrieve the role list [GET]

Lists all the roles that can be granted to an IAM Role or User Group on a Safe Deposit Box.

+ Response 200 (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

    + Body

            [
                {
                    "id": "f7fff4d6-faaa-11e5-a8a9-7fa3b294cd46",
                    "name": "owner",
                    "created_ts": "2016-04-05T04:19:51Z",
                    "last_updated_ts": "2016-04-05T04:19:51Z",
                    "created_by": "system",
                    "last_updated_by": "system"
                },
                {
                    "id": "f80027ee-faaa-11e5-a8a9-7fa3b294cd46",
                    "name": "write",
                    "created_ts": "2016-04-05T04:19:51Z",
                    "last_updated_ts": "2016-04-05T04:19:51Z",
                    "created_by": "system",
                    "last_updated_by": "system"
                },
                {
                    "id": "f800558e-faaa-11e5-a8a9-7fa3b294cd46",
                    "name": "read",
                    "created_ts": "2016-04-05T04:19:51Z",
                    "last_updated_ts": "2016-04-05T04:19:51Z",
                    "created_by": "system",
                    "last_updated_by": "system"
                }
            ]

# Group category

## Category List [/v1/category]

### Retrieve the category list [GET]

Lists all the possible categories that a safe deposit box can belong to.

+ Response 200 (application/json)

    + Headers

            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310

    + Body

            [
                {
                    "id": "f7ff85a0-faaa-11e5-a8a9-7fa3b294cd46",
                    "display_name": "Applications",
                    "path": "app",
                    "created_ts": "2016-04-05T04:19:51Z",
                    "last_updated_ts": "2016-04-05T04:19:51Z",
                    "created_by": "system",
                    "last_updated_by": "system"
                },
                {
                    "id": "f7ffb890-faaa-11e5-a8a9-7fa3b294cd46",
                    "display_name": "Shared",
                    "path": "shared",
                    "created_ts": "2016-04-05T04:19:51Z",
                    "last_updated_ts": "2016-04-05T04:19:51Z",
                    "created_by": "system",
                    "last_updated_by": "system"
                }
            ]

## User Login [/v2/auth/mfa_check]

### Authenticate with Cerberus as a User [POST]

This endpoint will take a Users credentials and proxy the request to Vault to get a Vault token for the user with some extra metadata.

+ Request (application/json)

    + Headers

            Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=

+ Response 200 (application/json)

    + Body

            {
               "status": "success",
               "data": {
                   "client_token": {
                       "client_token": "7f6808f1-ede3-2177-aa9d-45f507391310",
                       "policies": [
                           "web",
                           "stage"
                       ],
                       "metadata": {
                           "username": "john.doe@nike.com",
                           "is_admin": "false",
                           "groups": "Lst-CDT.CloudPlatformEngine.FTE,Lst-digital.platform-tools.internal"
                       },
                       "lease_duration": 3600,
                       "renewable": true
                   }
               }
            }
            
# Group Stats

## Basic Stats [/v1/stats]

### Get stats [GET]

Returns basic stats about each safe deposit box (name, owner, last updated ts). Requester must be an admin.

+ Response 200 (application/json)

    + Headers
    
            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310
        
    + Body
    
            {
              "safe_deposit_box_stats": [
                {
                  "name": "Web",
                  "owner": "Lst-CDT.CloudPlatformEngine.FTE",
                  "last_updated_ts": "2016-05-18T06:51:08Z"
                },
                {
                  "name": "OneLogin",
                  "owner": "Lst-CDT.CloudPlatformEngine.FTE",
                  "last_updated_ts": "2016-05-18T06:35:55Z"
                }
              ],
              "safe_deposit_box_total": 2
            }
            
# Group Metadata

## SDB Metadata [/v1/metadata]

### Get metadata [GET]

Returns pageable metadata for all SDBs
You can use has_next and next_offset from the response to paginate through all records

+ Parameters
    + limit (number) - OPTIONAL: The number of records to include in the metadata result. Defaults to 100
    + offset (number) - OPTIONAL: The offset to use when paginating records. Defaults to 0

+ Response 200 (application/json)

    + Headers
    
            X-Vault-Token: 7f6808f1-ede3-2177-aa9d-45f507391310
        
    + Body
    
            {
                "has_next": false,
                "next_offset": 10,
                "limit": 10,
                "offset": 0,
                "sdb_count_in_result": 10,
                "total_sdbcount": 1000,
                "safe_deposit_box_meta_data": [
                    {
                        "name": "dev demo",
                        "path": "app/dev-demo/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "test\nasdfasdasdfasd\nasdfasdf\n\nasdfasdf\nasdf",
                        "created_ts": "2017-01-04T23:18:40-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:18:40-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Application.FOO.User": "read",
                            "Application.BAR.User": "read"
                        },
                        "iam_role_permissions": {
                            "arn:aws:iam::265866363820:role/asdf": "write",
                            "arn:aws:iam::265866363820:role/fasdf": "write"
                        }
                    },
                    {
                        "name": "nike dev foo bar",
                        "path": "app/nike-dev-foo-bar/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "adsfasdfadsfasdf",
                        "created_ts": "2017-01-04T23:19:03-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:19:03-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Lst-FOO-bar": "read"
                        },
                        "iam_role_permissions": {}
                    },
                    {
                        "name": "IaM W d WASD",
                        "path": "shared/iam-w-d-wasd/",
                        "category": "Shared",
                        "owner": "Lst-Squad.Carebears",
                        "description": "CAREBERS",
                        "created_ts": "2017-01-04T23:19:19-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:19:19-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {},
                        "iam_role_permissions": {}
                    },
                    {
                        "name": "dev demo",
                        "path": "app/dev-demo/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "test",
                        "created_ts": "2017-01-04T23:18:40-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:18:40-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Application.FOO.User": "read"
                        },
                        "iam_role_permissions": {
                            "arn:aws:iam::265866363820:role/asdf": "write"
                        }
                    },
                    {
                        "name": "nike dev foo bar",
                        "path": "app/nike-dev-foo-bar/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "adsfasdfadsfasdf",
                        "created_ts": "2017-01-04T23:19:03-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:19:03-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Lst-FOO-bar": "read"
                        },
                        "iam_role_permissions": {}
                    },
                    {
                        "name": "IaM W d WASD",
                        "path": "shared/iam-w-d-wasd/",
                        "category": "Shared",
                        "owner": "Lst-Squad.Carebears",
                        "description": "CAREBERS",
                        "created_ts": "2017-01-04T23:19:19-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:19:19-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {},
                        "iam_role_permissions": {}
                    },
                    {
                        "name": "dev demo",
                        "path": "app/dev-demo/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "test",
                        "created_ts": "2017-01-04T23:18:40-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:18:40-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Application.FOO.User": "read"
                        },
                        "iam_role_permissions": {
                            "arn:aws:iam::265866363820:role/asdf": "write"
                        }
                    },
                    {
                        "name": "nike dev foo bar",
                        "path": "app/nike-dev-foo-bar/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "adsfasdfadsfasdf",
                        "created_ts": "2017-01-04T23:19:03-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:19:03-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Lst-FOO-bar": "read"
                        },
                        "iam_role_permissions": {}
                    },
                    {
                        "name": "IaM W d WASD",
                        "path": "shared/iam-w-d-wasd/",
                        "category": "Shared",
                        "owner": "Lst-Squad.Carebears",
                        "description": "CAREBERS",
                        "created_ts": "2017-01-04T23:19:19-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:19:19-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {},
                        "iam_role_permissions": {}
                    },
                    {
                        "name": "dev demo",
                        "path": "app/dev-demo/",
                        "category": "Applications",
                        "owner": "Lst-Squad.Carebears",
                        "description": "test",
                        "created_ts": "2017-01-04T23:18:40-08:00",
                        "created_by": "justin.field@nike.com",
                        "last_updated_ts": "2017-01-04T23:18:40-08:00",
                        "last_updated_by": "justin.field@nike.com",
                        "user_group_permissions": {
                            "Application.FOO.User": "read"
                        },
                        "iam_role_permissions": {
                            "arn:aws:iam::265866363820:role/asdf": "write"
                        }
                    }                                      
                ]
            }            