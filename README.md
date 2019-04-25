How To receive the message from RabbitMQ
------------------------------------------
step-1
--------
create spring boot project with RabbitMQ dependency

Example:
-------

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-amqp</artifactId>
		</dependency>
		

step-2
------

create following beans
----------------------
1-ConnectionFactory --- for the connection of rabbitmq <br>
2-SimpleMessageListenerContainer --for listening the associated queue.<br>
3-MessageListenerAdapter --for mapping the receiver method to receive message<br>

Example:
-------
	@Bean
	public ConnectionFactory connectionFactory() {
		CachingConnectionFactory factory =  new CachingConnectionFactory("localhost");
		factory.setPort(5672);
		factory.setUsername("guest");
		factory.setPassword("guest");
		return factory;
	}
	
	@Bean
    SimpleMessageListenerContainer container(ConnectionFactory connectionFactory,
            MessageListenerAdapter listenerAdapter) {
        SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
        container.setConnectionFactory(connectionFactory);
        container.setQueueNames("DIRECT-QUEUE");
        container.setMessageListener(listenerAdapter);
        return container;
    }

    @Bean
    MessageListenerAdapter listenerAdapter(Receiver receiver) {
        return new MessageListenerAdapter(receiver, "receiveMessage");
    }
    

 