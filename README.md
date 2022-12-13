# native-envoy-sandbox

### Strategy
##### Phase 01
1. While using curiefense UI, user will be able to render security policy in a yaml / protobuf, pushing it as an xDS update to their Envoy.
2. Rate Limits will be translated as Envoy Native RL
3. UI will tell wether a certain policy can be used as such, or not

##### Phase 02
1. deeper integrations with Gateways, and other Envoy based products


#### What's Done

* Added support for the following Curiefense policy generation
	* WAF rules (signatures)
	* Global filters tagging
	* ACL
  
  
#### Capabilities map

| Function          | y/n | Remarks                        |
|-------------------|-----|--------------------------------|
| Match headers     | Yes |                                |
| Match CIDR        | Yes |                                |
| Query string args | No  |                                |
| Body args         | No  | form-url-encoded and multipart |
| JSON body         | No  | nested structures              |
| GraphQL body      | No  |                                |
| XML body          | No  |                                |
| Geo, ASN, IP INfo | No  | MaxMind integration            |
