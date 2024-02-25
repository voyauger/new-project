#!/bin/bash

# Check if the correct number of arguments are passed
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <namespace> <resource_type>"
    exit 1
fi

NAMESPACE=$1
RESOURCE_TYPE=$2

# Retrieve resource usage statistics from Kubernetes
kubectl get $RESOURCE_TYPE -n $NAMESPACE | tail -n +2 | while read line
do
  # Extract CPU and memory usage from the output
  NAME=$(echo $line | awk '{print $1}')
  CPU=$(echo $line | awk '{print $2}')
  MEMORY=$(echo $line | awk '{print $3}')

  # Output the statistics to the console
  echo "Resource: $RESOURCE_TYPE, Namespace: $NAMESPACE, Name: $NAME, CPU: $CPU, Memory: $MEMORY"
done

