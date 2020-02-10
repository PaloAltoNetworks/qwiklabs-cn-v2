# CN-Series Next-Generation Firewall Deployment

This is a repository for YAMLs to deploy CN-Series Next-Generation firewall from Palo Alto Networks.

All the YAMLs required to deploy CN-Series on a given cloud platform are present under that cloud platform specific directory. Users can use these YAMLs as is to deploy CN-Series quickly after filling in just these fields from their setup:
  ```
  In pan-cni.yaml, pan-cn-mgmt.yaml and pan-cn-ngfw.yaml:
      image: <your-private-registry-image-path>

  In pan-cn-mgmt-configmap.yaml:
      # Panorama settings
      PAN_PANORAMA_IP: <panorama-IP>
      PAN_PANORAMA_AUTH_KEY: <panorama-auth-key>
      PAN_DEVICE_GROUP: <panorama-device-group>
      PAN_TEMPLATE: <panorama-template-stack>
  ```
For production deployment, it's expected users would want to customize the YAMLs as per below:
  - Resources (cpu, memory) fields in pan-cn-mgmt.yaml and pan-cn-ngfw.yaml are pre-populated but should be customized to better suit the deployment scenario.
  - There are some optional fields in the configmaps which users can add e.g. PAN_PANORAMA_CGNAME for Collector Group name or PAN_PANORAMA_IP2 for Panorama in HA.
  **Note**: For complex setup and advanced topics needing modifications in the YAMLs, refer to the deployment documentations for details. Changing a field might require modification in multiple places and multiple YAMLs.


Once the YAMLs have been modified as desired, these YAMLs can be deployed as:
``` 
kubectl apply -f plugin-serviceaccount.yaml
kubectl apply -f pan-serviceaccount.yaml
kubectl apply -f pan-cni-configmap.yaml
kubectl apply -f pan-cni.yaml
kubectl apply -f pan-cn-mgmt-configmap.yaml
kubectl apply -f pan-cn-mgmt.yaml
kubectl apply -f pan-cn-ngfw-configmap.yaml
kubectl apply -f pan-cn-ngfw.yaml
```

To enable the security for the application pods, apply the following annotation to their YAMLs, OR, to enable the security for all the pods in a given namespace, apply this annotation to the namespace: ```     paloaltonetworks.com/firewall: pan-fw``` e.g. for "default" namespace 
```
kubectl annotate namespace default paloaltonetworks.com/firewall=pan-fw
```
Refer to the deployment documentations for more details on it.

**Documentation**

- [CN-Series Deployment](<https://docs.paloaltonetworks.com/vm-series/9-1/vm-series-deployment>)
- [CN-Series Datasheet](<https://www.paloaltonetworks.com/resources/datasheets/vm-series-specsheet>)

# Support Policy
**Community-Supported aka Best Effort:

These Kubernetes YAML files are released under an as-is, best effort, support policy. These scripts should be seen as community supported and Palo Alto Networks will contribute our expertise as and when possible. We do not provide technical support or help in using or troubleshooting the components of the project through our normal support options such as Palo Alto Networks support teams, or ASC (Authorized Support Centers) partners and backline support options. The underlying product used (the VM-Series firewall) by the scripts or templates are still supported, but the support is only for the product functionality and not for help in deploying or using the template or script itself. Unless explicitly tagged, all projects or work posted in our GitHub repository (at https://github.com/PaloAltoNetworks) or sites other than our official Downloads page on https://support.paloaltonetworks.com are provided under the best effort policy.
