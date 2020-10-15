//using monitor
monitor DP 
{ 
	status state[5]; 
	condition self[5]; 

	Pickup(int i) 
	{ 
		state[i] = hungry; 

		if (state[i] != eating) 
			self[i].wait; 
	} 

	Putdown(int i) 
	{ 
		state[i] = thinking; 

		test((i + 1) % 5); 
		test((i + 4) % 5); 
	} 

	test(int i) 
	{ 

		if (state[(i + 1) % 5] != eating 
			&& state[(i + 4) % 5] != eating 
			&& state[i] == hungry) { 

			state[i] = eating; 

			self[i].signal(); 
		} 
	} 

	init() 
	{ 
for
	i = 0 to 4 
		state[i] = thinking; 
	} 
}
