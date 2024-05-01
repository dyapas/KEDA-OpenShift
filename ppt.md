## AutoScaling - Introduction

- Workloads deployed on OpenShift/k8s cluster should be able to flexibly react to increased/decreased load To prevent any outages, Latency issues, and avoid resource wastage.
- Can be achieved by 
  - Application AutoScaling 
  - Cluster AutoScaling
Cluster AutoScaling:
  - The cluster autoscaler watches for pods that can't be scheduled on nodes because of resource constraints. 
  - The cluster then automatically increases the number of nodes and removes the nodes when load is low.
**Application AutoScaling**
  - Vertical Pod Autoscaler to scale pods vertically (change the resource allocation for a pod)
  - Horizontal Pod Autoscaler to scale pods horizontally (change the number of pods)
  - Scale workloads based on resource utilization metrics, ie. CPU and memory usage
  - Scaling based only on CPU and Memory consumption might not always be the best fit.
- Modern applications workloads leverages Event Driven Architecture and traditional VPA/HPA may not work.
- Wanted to scale applications based on events/metrics from external systems other than CPU and MEMORY.

## Custom Metrics AutoScaler ( CMA )
- The Custom Metrics AutoScaler ( CMA ) automatically increase or decrease the number of pods for a deployment, statefulset, custom resource, or job based on custom metrics that are not based only on CPU or memory.
- The Custom Metrics Autoscaler operator is based on the upstream project  KEDA .
- KEDA is a Kubernetes-based Event Driven Autoscaling component. It provides event driven scale for any container running in Kubernetes
- KEDA is CNCF Graducated project.
- RedHat is one of the Co-Founder of KEDA along with MicroSoft.
- The main goal of CMA/KEDA is to enable autoscaling based on events and custom metrics other than pod metrics. 
- Acts as a thin layer on top of the existing Horizontal Pod AutoScaler, it provides it with external metrics and also manages scaling down to zero
- CMA currently supports only the Prometheus, CPU, memory, and Apache Kafka metrics.
- The CMA uses a metrics API to convert the external metrics to a form that OpenShift Container Platform can use. The custom metrics autoscaler creates a horizontal pod autoscaler (HPA) that performs the actual scaling.
### Pre-requisites:
  - Enable monitoring for user-defined projects
  - Expose metrics from the application at /metrics endpoint ( using ServiceMonitor resource )

**Components of CMA**
  - **Triggers/Scalers**: source of events and metrics that the custom metrics autoscaler uses to determine how to scale.
  - **ScaledObject**: custom resource (CR) that define autoscaling of Deployments, StatefulSets or any Custom Resources that are scalable.
  - **ScaledJob**: custom resource (CR) to process long running tasks which the autoscaler uses to create Kubernetes Jobs based on the custom metrics.
- Only one scaled object or scaled job is allowed for each workload that we want to scale
- We cannot use a scaled object or scaled job and the horizontal pod autoscaler (HPA) on the same workload






