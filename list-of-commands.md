aws cloudformation deploy --template-file cfn-new-product.yaml --stack-name new-product-stack --capabilities CAPABILITY_NAMED_IAM

aws cloudformation deploy --template-file cfn-legacy-product.yaml --stack-name product-legacy --parameter-overrides "$(cat param-legacy-product.json)" --capabilities CAPABILITY_IAM

aws cloudformation deploy --template-file cfn-lattice-basic.yaml --stack-name vpc-lattice-stack --parameter-overrides "$(cat param-lattice.json)"   