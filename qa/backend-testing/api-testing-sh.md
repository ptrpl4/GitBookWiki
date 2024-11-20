# API testing scripts

test.sh
```sh
#!/bin/bash

BASE_URL=${BASE_URL:-'https://wallet.local'}
USER_ID=${USER_ID:-'5ca4nf02-7a8b-46cf-bc6a-636ff1dds'}
EXPECTED_ID=${EXPECTED_ID:-123}

echo "Starting test..."

getLastPaymentAccountIDByUserUUID_test() {
    URL="$BASE_URL/api/v1/user/$USER_ID/payment_account"
    RESPONSE=$(curl -s -k "$URL")
    STATUS_CODE=$(curl -s -o /dev/null -w "%{http_code}" -k "$URL")
    ID=$(echo "$RESPONSE" | jq -r '.id')

    echo "Request completed with status code $STATUS_CODE"

    if [ "$STATUS_CODE" -eq 200 ] && [ "$ID" -eq "$EXPECTED_ID" ]; then
        echo "Test passed: ID matches expected value"
    else
        echo "Test failed! Status code: $STATUS_CODE, ID: $ID, Expected ID: $EXPECTED_ID"
        exit 1
    fi
}

getLastPaymentAccountIDByUserUUID_test

echo "Test completed"
```

dockerfile
```dockerfile
# Use a lightweight base image
FROM alpine:latest

# Install necessary packages
RUN apk add --no-cache bash curl jq

# Copy the script into the container
COPY test.sh /usr/local/bin/test.sh

# Make the script executable
RUN chmod +x /usr/local/bin/test.sh

# Set the entrypoint to run the script
ENTRYPOINT ["/usr/local/bin/test.sh"]
```