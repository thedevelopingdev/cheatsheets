# AWS

## Deep learning

### GPU instances
- A100 (8)
- V100 (8)
- A10G (8)
- T4 (4)
- M60 (4)

### Pricing

#### [ECR](https://aws.amazon.com/ecr/pricing/)
- Storage cost: 0.10 per GB per month
- Data transfer out of ECR (and not in same region): 0.09 per GB

##### Example scenarios
- Training and storing on EC2
    - PyTorch container: ~10 GB
    - Likely working with max of 10 different containers
    - Data transfer cost: free
    - On-demand `training_cost_per_session`
        - 1 `g5.xlarge` instance (1x A10G_24GB)
        - training time: 6 hrs
        - setup time: 1 hr
        - Training cost: `$1 / hr / instance * (6 + 1 hr) = $7 / session`
    - Storage cost: `100 GB * 0.10 / GB / month`
    - Total: `$10 / month (storage) + $7 [training] * training_sessions_per_month`
- Training on Lambda cloud, storing images on ECR
    - 4 machine cluster (=4 pulls per training session; 40 GB)
    - Data transfer cost: `$4 / training session * training_sessions_per_month`
    - Storage cost: `100 GB * 0.10 / GB / month`
    - On-demand `training_cost_per_session`
        - 1 `A10_24GB` instance
        - training time: 6 hrs
        - setup time: 1 hr
        - Training cost: `$0.6 / hr / instance * (6 + 1 hr) = $4.2 / session`
    - Total: `$10 / month [storage] + (4 [data xfr] + 4.2 [training]) * training_sesions_per_month`
- Training and storing on Lambda cloud
    - Storage cost: `100 GB * 0.20 / GB / month = $20 / month`
    - On-demand `training_cost_per_session`
        - 1 `A10_24GB` instance
        - training time: 6 hrs
        - setup time: 1 hr
        - Training cost: `$0.6 / hr / instance * (6 + 1 hr) = $4.2 / session`
    - Total: `$20 / month [storage] + 4.2 [training] * training_sesions_per_month`
- Takeaways
    - Lambda Cloud (storage+training): save if running 4 or more training sessions per month
    - Lambda Cloud (training), AWS (storage): if running 2 training sessions per month
    - AWS-ondemand (storage+training): if running 1 training session per month


### AMIs for deep learning
- https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html
  - https://aws.amazon.com/releasenotes/aws-deep-learning-base-gpu-ami-ubuntu-20-04/
  - https://aws.amazon.com/releasenotes/aws-deep-learning-ami-gpu-pytorch-2-0-ubuntu-20-04/

### Instance types for deep learning
- https://aws.amazon.com/ec2/instance-types/
