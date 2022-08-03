#!/bin/bash
<<Documentation
Name: Manideep Duggi
Date: 27-07-2022 
Description: To Perform Command line test
Sample I/P: (Command line)
Sample O/P: Performs Test.
Documentation
flag=20
while [ $flag = 20 ]
do
	clear      
	flag=0
	correct=0
	while [ $flag -eq 0 ]
	do
		clear
		echo -----------------------------------------------------------------------------------------------------------------------------------------------------
		echo "                                                                # W E L C O M E #                                                           "
		echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
		echo 1.Sign up
		echo 2.Sign in
		echo 3.Exit
		read -p "Enter the choice " choice
		user=(`cat user.csv`)
		password=(`cat password.csv`)
		case $choice in
			1)
				#signup
				clear
				echo -----------------------------------------------------------------------------------------------------------------------------------------------------
				echo "                                                                    # Sign up #                                                           "
				echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
				echo "Enter user name:"
				read user_name
				for i in ${user[@]}
				do
					if [ $user_name = $i ]
					then
						flag=0
						echo "User Id already exists try anoyther user name"
						break
					else
						flag=1
					fi
				done
				if [ $flag -eq 1 ]
				then
					echo "$user_name" >> user.csv
					flag=2
				fi
				while [ $flag = 2 ]
				do
					echo "Enter password"
					read -s password
					echo "Confirm password"
					read -s password1
					if [ $password = $password1 ]
					then
						echo "$password1" >> password.csv
						flag=3
					else
						echo "Error:password is not matching"
						echo "Enter the correct password"
						flag=2
					fi
				done
				clear
				echo -----------------------------------------------------------------------------------------------------------------------------------------------------
				echo
				echo
				echo "                                                           # Registered in successfully #                                                           "
				echo
				echo
				echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
				sleep 2
				flag=0
				;;
			2)
				#sign in
				rm user_answer.txt
				touch user_answer.txt
				clear
				echo -----------------------------------------------------------------------------------------------------------------------------------------------------
				echo "                                                                    # Sign in #                                                           "
				echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
				flag=8
				while [ $flag = 8 ]
				do
					echo "Enter user name"
					read user_name1
					len1=$((${#user[@]}-1))
					flag=8
					for i in `seq 0 $len1`
					do
						if [ $user_name1 = ${user[$i]} ]
						then
							echo "Enter password"
							read -s password3
							if [ $password3 = ${password[$i]} ]
							then
								flag=4
								break
							else
								flag=8
							fi
						else
							flag=8
						fi
					done
					if [ $flag = 8 ]
					then
						echo "Please enter valid user Name or Password"
					fi
				done
				if [ $flag = 4 ]
				then
					clear
					echo -----------------------------------------------------------------------------------------------------------------------------------------------------
					echo
					echo
					echo "                                                           # Logged in successfully #                                                           "
					echo
					echo
					echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
					sleep 1
					flag=9
					while [ $flag = 9 ]
					do
						clear
						echo -----------------------------------------------------------------------------------------------------------------------------------------------------
						echo "                                                                # W E L C O M E #                                                           "
						echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
						echo "Enter choice"
						echo "1.Take test"
						echo "2.Log out"
						read test
						case $test in
							1)
								echo "Take test"
								for j in `seq 5 5 25`
								do
									clear
									echo -----------------------------------------------------------------------------------------------------------------------------------------------------
									echo "                                                           # A L L  T H E  B E S T  #                                                           "
									echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
									cat questions.txt | head -$j | tail -5
									for i in `seq 10 -1 1`
									do
										flag=6
										echo -e "\r choose option:$i \c"
										read -t 1 opt
										if [ ${#opt} -eq 1 ]
										then
											echo "$opt" >> user_answer.txt
											break
										else
											flag=5
										fi
									done
									if [ $flag -eq 5 ]
									then
										opt=e
										echo "$opt" >> user_answer.txt
									fi
								done
								clear
								echo -----------------------------------------------------------------------------------------------------------------------------------------------------
								echo "                                                              # A N S W E R S #                                                           "
								echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
								#comparing answers
								user_answer=(`cat user_answer.txt`)
								correct_answers=(`cat correct_answers.txt`)
								len=$((${#correct_answers[@]}-1))
								head=5
								correct=0
								tout=0
								wrong=0
								for comp in `seq 0 $len`
								do
									cat questions.txt | head -$head | tail -5
									if [ ${user_answer[$comp]} = e ]
									then
										echo -e "\e[33mTIMEOUT"
										echo -e "\e[0m" " "
										echo -e "\e[32mcorrect answer is ${correct_answers[$comp]}"
										echo -e "\e[0m" " "
										tout=$(($tout+1))

									elif [ ${correct_answers[$comp]} = ${user_answer[$comp]} ]
									then
										echo -e "\e[32mcorrect answer"
										echo -e "\e[0m" " "
										echo -e "\e[32mcorrect answer is ${correct_answers[$comp]}"
										echo -e "\e[0m" " "
										correct=$(($correct+1))
									else 
										echo -e "\e[31mwrong answer"
										echo -e "\e[0m" " "
										echo -e "\e[32mcorrect answer is ${correct_answers[$comp]}"
										echo -e "\e[0m" " "
										wrong=$(($wrong+1))
									fi
									head=$(($head+5))
								done
								echo -----------------------------------------------------------------------------------------------------------------------------------------------------
								echo "                                                                # R E S U L T #                                                           "
								echo "-----------------------------------------------------------------------------------------------------------------------------------------------------"
								echo -e "\e[32mTotal correct answers are $correct"
								echo -e "\e[0m" " "
								echo -e "\e[33mTotal wrong answers are $wrong"
								echo -e "\e[0m" " "
								echo -e "\e[31mTotal Timeout are $tout"
								echo -e "\e[0m" " "
								flag=10
								read -p "Press q and Enter to quit" out
								if [ $out = q -o $out = Q ]
								then
									clear
									exit
								else
									flag=20
								fi
								;;
							2)
								echo "logged out"
								flag=0
								;;
							*)
								echo "Error:You Entered invalid option!!!"
								flag=9
						esac
					done
				fi
				;;
			3)
				clear
				exit
				;;
			*)
				echo "Error:please enter correct choice(1,2,3)"

			esac
		done
	done
