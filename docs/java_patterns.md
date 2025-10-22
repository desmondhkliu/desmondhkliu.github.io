	1. Singleton Pattern
	A singleton ensures that only one instance of a class exists throughout the application lifecycle. It’s often used for shared resources like configuration, logging, or connection pools.
	<ol>

		<li>Thread-Safety<br/>
			In multi-threaded environments, multiple threads might try to create the singleton simultaneously — leading to multiple instances or race conditions.<br/>
			To achieve:<br/>
			<ul>
				<li>Synchronized blocks (classic but slower)</li>
				<li>Double-checked locking (efficient and safe)</li>
				<li>Static holder idiom (best for lazy + thread-safe)</li>
				<li>Enum singleton (guaranteed by JVM)</li>
			</ul>

		</li>
	</ol>
