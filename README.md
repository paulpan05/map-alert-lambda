# Map Alert Lambda Back-end

To run the function, do the following:

```sh
aws --endpoint-url http://localhost:4566 iam create-role \
--role-name lambda-cpp-demo \
--assume-role-policy-document file://trust-policy.json

aws --endpoint-url http://localhost:4566 lambda create-function \
--function-name hello-world \
--role arn:aws:iam::000000000000:role/lambda-cpp-demo \
--runtime provided \
--timeout 15 \
--memory-size 128 \
--handler hello \
--zip-file fileb://hello.zip

aws --endpoint-url http://localhost:4566 lambda invoke \
--function-name hello-world \
--payload '{ }' \
output.txt
```

To update function code:

```sh
aws --endpoint-url http://localhost:4566 lambda update-function-code \
--function-name hello-world \
--zip-file fileb://hello.zip
```

To build project, create build directory and ```cd``` into the directory, then run:
```sh
cmake3 .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=~/out
```