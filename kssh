#!/bin/bash

# ssm-connect: Connect to EC2 instance via SSM using a hostname/tag-based lookup

set -e

if [ -z "$1" ]; then
    echo "Usage: ssm-connect <hostname>"
    exit 1
fi

HOSTNAME=$1
TAG_KEY=""

# Determine the correct tag key based on hostname prefix
if [[ $HOSTNAME == ecliwb* ]]; then
    TAG_KEY="Fe_Name"
elif [[ $HOSTNAME == eclidb* ]]; then
    TAG_KEY="Be_Name"
elif [[ $HOSTNAME == ftp* ]]; then
    TAG_KEY="Name"
    HOSTNAME="$HOSTNAME*"
else
    echo "❌ Error: Hostname prefix not recognized. Must start with 'ecliwb', 'eclidb', or 'ftp'"
    exit 1
fi

echo "🔍 Looking for EC2 instance with tag $TAG_KEY=$HOSTNAME..."

INSTANCE_ID=$(aws ec2 describe-instances \
    --filters "Name=tag:$TAG_KEY,Values=$HOSTNAME" \
    --query "Reservations[*].Instances[*].InstanceId" \
    --output text)

if [ -z "$INSTANCE_ID" ]; then
    echo "❌ Error: No matching instance found for hostname '$HOSTNAME'"
    exit 1
fi

echo "✅ Instance found: $INSTANCE_ID"
echo "🚀 Starting SSM session..."
aws ssm start-session --target "$INSTANCE_ID"
