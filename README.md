## Notification Service

processAuction will sends to Simple Queue Service and this in turn calls a sendMail Lambda which passes email to Simple Email Service

sendMail Lambda set up, here sendMail event's come from mailQueue arn 
```
functions:
  sendMail:
    handler: src/handlers/sendMail.handler
    events:
      - sqs:
          arn: ${self:custom.mailQueue.arn}
          batchSize: 1
```

closedAuction from Auction service calls SQS

```
  const notifySeller = sqs.sendMessage({
    QueueUrl:process.env.MAIL_QUEUE_URL,
    MessageBody: JSON.stringify({
      subject:'Your item has been sold',
      recipient:seller,
      body:`Congratulations! You item has been ${title} has been sold for $${amount}`,
    })
  }).promise()

  const notifyBidder = sqs.sendMessage({
    QueueUrl:process.env.MAIL_QUEUE_URL,
    MessageBody: JSON.stringify({
      subject:'You won an auction!',
      recipient:bidder,
      body:`Congratulations! you got won the bid for ${title} for $${amount}`,
    })
  }).promise()
```


## Getting started
```
npm install
```

You are ready to go!
