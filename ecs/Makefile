prepare:
	export AWS_DEFAULT_REGION=us-east-1
	export ROLE_NAME=ecsMithrilRole
	export CLUSTER_NAME=test-ecs-anywhere
	export SERVICE_NAME=test-ecs-anywhere-svc

# NOTE Refactor to use terraform ;)
policy-rollout:
	aws iam create-role --role-name $ROLE_NAME --assume-role-policy-document file://ssm-trust-policy.json
	aws iam attach-role-policy --role-name $ROLE_NAME --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
	aws iam attach-role-policy --role-name $ROLE_NAME --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceforEC2Role

policy-verify:
	aws iam list-attached-role-policies --role-name $ROLE_NAME

cluster-create:
	aws ecs create-cluster --cluster-name $CLUSTER_NAME
	aws ssm create-activation --iam-role $ROLE_NAME | tee ssm-activation.json

cluster-verify:
	aws ssm describe-instance-information
	aws ecs list-container-instances --cluster $CLUSTER_NAME

cluster-task-register:
	aws ecs register-task-definition --cli-input-json file://external-task-definition.json
	#Run the task
	aws ecs run-task --cluster $CLUSTER_NAME --launch-type EXTERNAL --task-definition nginx

cluster-task-verify:
	#Get the Task ID
	TEST_TASKID=$(aws ecs list-tasks --cluster $CLUSTER_NAME | jq -r '.taskArns[0]')

	#Verify Task is Running
	aws ecs describe-tasks --cluster $CLUSTER_NAME --tasks $TEST_TASKID

	open http://<your VM IP>:8080

cleanup:
	# Cleanup SSM
	aws ssm describe-activations | jq ".ActivationList | .[] | .ActivationId" | xargs -L 1 aws ssm delete-activation --activation-id
	aws ssm describe-instance-information | jq ".InstanceInformationList | .[] | .InstanceId" | grep "mi-" | xargs -L 1 aws ssm deregister-managed-instance --instance-id

	# Cleanup ECS resources
	aws ecs list-container-instances --cluster $CLUSTER_NAME | jq ".containerInstanceArns | .[]" | xargs -L 1 aws ecs deregister-container-instance --cluster $CLUSTER_NAME --force --container-instance

	aws ecs delete-cluster --cluster $CLUSTER_NAME

	# Verify all items deleted are empty
	aws ssm describe-activations
	aws ssm describe-instance-information
	aws ecs list-container-instances --cluster $CLUSTER_NAME

	#Remove Multipass VM (optional)
	multipass stop ecs-anywhere-ubuntu
	multipass delete ecs-anywhere-ubuntu
