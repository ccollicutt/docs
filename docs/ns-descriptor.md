# Network Service Descriptor

The Network Service Descriptor is a template file, whose parameters are following the [ETSI MANO specification][nfv-mano], used by the NFV Orchestrator (NFVO) for deploying network services (as combination of multiple VNFs). There are two different formats supported by the NFVO for NSD:

* JSON file representation of the information model specificied by the [ETSI MANO specification][nfv-mano]
* TOSCA compliant Network Service Template on boarded via CSAR archive compliant with the [TOSCA Simple Profile for NFV][tosca].

This page provide further details about option 1 (which is at the moment the more advanced feature-wise API offered by the NFVO). Refer to the [NS Template tutorial][ns-template] and [CSAR Onboarding tutorial][csar-onboard] for examples about TOSCA.

## Overview

Below you can find an example of a NSD for deploying 1..n VNFs:

```
{  
	"name":"iperf-NSD",
    "vendor":"fokus",
    "version":"0.1-ALPHA",
    "vnfd":[  ...  ],
    "vld":[  
        {  
            "name":"private"
        }
    ],
    "vnf_dependency":[
    	{
      		"source" : {
       			"name": "iperf-server"
      		},
      		"target":{
        		"name": "iperf-client"
      		},
      		"parameters":[
        		"private"
      		]
    	}
    ]
}
```

You can see the complete NSD file of this example [here][nsd-iperf].

| Params          				| Meaning       													|
| -------------   				| -------------													|
| name  						| The name of the Network Service |
| vendor 						| The vendor or provider of the Network Service      	|
| version 						| The version of the Network Service (can be any string)      	|
| vnfd 							| The list of Virtual Network Functions composing the Network Service (see [Virtual Network Function Descriptor][vnf-descriptor] for more details)      	|
| vld 							| The list of Virtual Links that are referenced by the VNF Descriptors in order to define network connectivity      	|
| vnf_dependency 				| The list of dependencies between Virtual Network Functions (**Not mandatory**, please check the [VNF Descriptor page](vnf-descriptor) in order to understand more about this topic, as you may not need to specifiy dependencies here if those are already provided in terms of requirements at the VNFD level)     	|

### VNF Descriptors

In the _vnfd_ field you must put a list of ids `[{"id":"id_1"},{"id":"id_2"},{"id":"id_2"}]`, corresponding to the ids of the VNF Descriptors already uploaded. Alternatively, you can put the full VNFD, if you don't have specific scripts to be executed. If you already have uploaded some VNF Packages, building this list can be annoying, so just go in the VNFD page of the dashboard, select all wanted VNFDs and click on the clipboard copy button on the top left. You will have in the clipboard the list of ids already pre-build.

![Multiple VNF selection][multi-vnfd-select]

### Virtual Link Descriptor

The Virtual Link Descriptor (VLD) describes Virtual Link requirements for connecting one or more VNFs together.
The VLD must contain a parameter _name_ with the value of a network that will be used by the VNFD. If the network exists, then this network will be used, if not a new one will be created.

### VNF Dependencies

A VNF Dependency is composed by:

| Params          				| Meaning       													|
| -------------   				| -------------													|
| source  						| The name of the Virtual Network Function that provides one or more parameters (see [Generic VNF Manager][vnfm-generic] and [VNF Parameters][vnf-parameters])|
| target 						| The name of the Virtual Network Function that requires one or more parameters	(see [Generic VNF Manager][vnfm-generic] and [VNF Parameters][vnf-parameters])|
| parameters					| The list of parameters provided by the *source* to the *target*     	|

It is possible to let the Orchestrator to calculate dependencies automatically by providing some parameters in the VNF Descriptor part. Please check the [VNF Descriptor page](vnf-descriptor)

### Network Service Onboarding

After saving the NSD (giving json as format to the file) you could easily upload it via the [Dashboard][dashboard] or the [Command Line Interface][cli].

<!---
References
-->

[vnf-descriptor]:vnf-descriptor
[vnfm-generic]:vnfm-generic
[vnf-parameters]:vnf-parameters
[nfv-mano]: http://www.etsi.org/deliver/etsi_gs/NFV-MAN/001_099/001/01.01.01_60/gs_NFV-MAN001v010101p.pdf
[nsd-iperf]:nsd-json-example.md
[multi-vnfd-select]:images/multi-vnfd-select.png
[tosca]:http://docs.oasis-open.org/tosca/tosca-nfv/v1.0/csd03/tosca-nfv-v1.0-csd03.pdf
[csar-onboard]:tosca-CSAR-onboarding
[ns-template]:tosca-nsd
[dashboard]:nfvo-how-to-use-gui
[cli]:nfvo-how-to-use-cli

<!---
Script for open external links in a new tab
-->
<script type="text/javascript" charset="utf-8">
      // Creating custom :external selector
      $.expr[':'].external = function(obj){
          return !obj.href.match(/^mailto\:/)
                  && (obj.hostname != location.hostname);
      };
      $(function(){
        $('a:external').addClass('external');
        $(".external").attr('target','_blank');
      })
</script>
