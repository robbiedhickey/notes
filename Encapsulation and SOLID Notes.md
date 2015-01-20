## Encapsulation

### Information Hiding/Implementation Hiding

* Not only about private/public variables. 
* Is also about hiding implementation details.
* Prefer 'Implementation Hiding' to be more explicit about its meaning


### Protection of invariants

* Ensuring that invalid states are impossible.
	* Preconditions and Postconditions. 

### Command Query Separation Principle

Your operations should be either commands or queries, never both. CQS not CQRS. CQRS is architectural. 

#### Command - has side effects

* Mutate State
* Can invoke queries (this is safe, but it is not safe to invoke commands from queries)

```C#
// What does it do? We assume it saves the order.
void Save(Order order);

// What does this do? Probably sends the message.
void Send(T message);

// What does it do? Someone foo and bar are being assocated with one another. 
void Associate(IFoo foo, IBar bar);

```
What is common? They all have a void return type. There is no reason to invoke a void method unless it has side effects. Commands are usually easy to identify. 

#### Query - returns data

* Do not mutate observable state
* Idempotent (operation should not change state of system regardless of how many times you execute it, asking the question should not change the answer)
* Safe to invoke

```C#
// Probably gets the orders. All or some?
Order[] GetOrders(int userId);

// Probably translates information in bar to IFoo
IFoo Map(Bar bar);

// T is generic type argument and it somehow returns an instance of T
T Create();
```

What is common? They all return something. 

Following these rules make code easier to reason about. 

#### Example

```C#
// Where is the query here?
// The Read method should be, but it looks like a command (void)
// The Save method looks like a query
public class FileStore
{
	public string WorkingDirectory {get;set;}
	
	public string Save(int id, string message)
	{
		var path = Path.Combine(this.WorkingDirectory, id + ".txt");
		File.WriteAllText(path, message);
		return path;
	}

	public event EventHandler<MessageEventArgs> MessageRead;

	public void Read(int id)
	{
		var path.Combine(this.WorkingDirectory, id + ".txt");
		var msg = File.ReadAllText(path);
		this.MessageRead(this, new MessageEventArgs { Message = msg });
	}
}
```

Make Read() a query, remove side effects (event)

```C#
public class FileStore
{
	public string WorkingDirectory {get;set;}
	
	public string Save(int id, string message)
	{
		var path = Path.Combine(this.WorkingDirectory, id + ".txt");
		File.WriteAllText(path, message);
		return path;
	}

	public string Read(int id)
	{
		var path.Combine(this.WorkingDirectory, id + ".txt");
		return File.ReadAllText(path);
	}
}
```

Make Save() a command. Cannot throw away useful information, so instead we create a query so the user can retrieve the filename if needed. 

```C#
public class FileStore
{
	public string WorkingDirectory {get;set;}
	
	public void Save(int id, string message)
	{
		var path = GetFileName(id);
		File.WriteAllText(path, message);
	}

	public string Read(int id)
	{
		var path = GetFileName(id);
		return File.ReadAllText(path);
	}

	public string GetFileName(int id)
	{
		return Path.Combine(this.WorkingDirectory, id + ".txt");
	}
}
```
Notice that it is OKAY to execute a query from a Command. This is because queries have no side effects!

We solved this problem by decomposing command and query responsibilities into smaller methods. 

### Postel's Law

