cmake_minimum_required(VERSION 3.31)
project(Philosophers C)

set(CMAKE_C_STANDARD 11)

add_executable(philo Philosophers/philo/include/philosophers.h Philosophers/philo/include/util.h Philosophers/philo/src/error.c Philosophers/philo/src/exit.c Philosophers/philo/src/init.c Philosophers/philo/src/main.c Philosophers/philo/src/monitor.c Philosophers/philo/src/routine.c Philosophers/philo/src/routine_util.c Philosophers/philo/src/util.c Philosophers/philo/Makefile)
add_executable(philo_bonus Philosophers/philo_bonus/include/philo_bonus.h Philosophers/philo_bonus/include/util.h Philosophers/philo_bonus/src/error.c Philosophers/philo_bonus/src/exit.c Philosophers/philo_bonus/src/fork.c Philosophers/philo_bonus/src/init.c Philosophers/philo_bonus/src/main.c Philosophers/philo_bonus/src/philo.c Philosophers/philo_bonus/src/util.c Philosophers/philo_bonus/Makefile)
