// Basic simulate imprecise measurement code in RobotC
int min_value = 0;
int max_value = 100;

// Experiment parameters
int total_trials = 1000;

// Random number storage.
int true_value;
int total_meas = 0;

/**********************************************************
** void generate_new_value
** Generates new true_value
**********************************************************/
void generate_new_value()
{
	// Random number parameter

	int temp_number = rand();

	int range = max_value - min_value;

	// Kludge to handle fact that RobotC does not have unsigned ints.
	temp_number = ( temp_number < 0 ) ? -1 * temp_number : temp_number;

	true_value = temp_number % range + min_value;
}

/**********************************************************
** int resolve_measurement( int meas_point )
** Resolve Measurement at set point.
** Inputs:
**   meas_point	Set point of measurment
** Outputs:
**   0						If true value is equal to meas_point
**   -1          If true value is greater then meas_point
**   1           If true value is less than meas_pont
** Global Interaction:
**   Keeps track of number of measurements in total_meas
**
**********************************************************/
int resolve_measurement( int meas_point ) {

	// Increment total number of guesses.
	total_meas++;

	if ( meas_point > true_value ) {
		// Return 1 if measurement point is too large.
		return(1);
		} else if ( meas_point < true_value ) {
		// Return -1 if measurment point is too small.
		return(-1);
		} else {
		// Return 0 if measurement point matches
		return(0);
	}
}

task main()
{
	// Run multiple trials
	int current_trial;

	// Range to search for true value
	int min_point = min_value;
	int max_point = max_value;

	// For each trial
	for ( current_trial = 0; current_trial < total_trials ; current_trial++ ) {

		// Generate new value to measure
		generate_new_value();

		// Modify the code starting here.
int a;
		a=(max_value+min_value)/2;// setting the variable a to the nearest point

		int current_meas_point = a;// assigning the value of the nearest point to the current_meas_point

		// Estimation code here.
		// Sweep through range from lowest to highest possible value.
		// Stop when measurment_point matches true value.
		int distance=resolve_measurement( current_meas_point );// calling the function to check the result
		while ( distance != 0  ) // this will run if the value of the result is not zero
			{
			if(distance==(1))    // if the value is 1
				{
				max_value=a-1;   // setting the max_value= a-1
			}
			else if(distance==(-1)) //if the value is 1
				{
				min_value=a+1;   // setting the max_value= a-1
			}
			a=(max_value+min_value)/2; // if the value already is zero then it will keep formualted value
			distance=resolve_measurement( a );// the program will be called if the result is 0

		}

		// All modified code should be above this line.

		// Write output to debug stream.
		// Keep this line for evaluation of performance.
		writeDebugStream("%d trials and %d guesses\n",current_trial,total_meas);

	}

	// Wait for an infinite amount of time.
	// Keeps the Debug stream open so we can read the output of the program.
	while(1);
}