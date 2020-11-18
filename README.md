# gostalkd-example

High available gostalkd.

## consumer example
```go
consumer := dq.NewConsumer(dq.DqConf{
	Beanstalks: []dq.Beanstalk{
		{
			Endpoint: "localhost:11400",
			Tube:     "tube",
		},
		{
			Endpoint: "localhost:11401",
			Tube:     "tube",
		},
	},
	Redis: redis.RedisConf{
		Host: "localhost:6379",
		Type: redis.NodeType,
	},
})
consumer.Consume(func(body []byte) {
	fmt.Println(string(body))
})
```

## producer example
```go
producer := dq.NewProducer([]dq.Beanstalk{
	{
		Endpoint: "localhost:11400",
		Tube:     "tube",
	},
	{
		Endpoint: "localhost:11401",
		Tube:     "tube",
	},
})	

for i := 1000; i < 1005; i++ {
	_, err := producer.Delay([]byte(strconv.Itoa(i)), time.Second*5)
	if err != nil {
		fmt.Println(err)
	}
}
```
