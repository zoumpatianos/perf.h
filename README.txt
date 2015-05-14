# perf.h
Perf header library for counting system events

/* ---------------------------------------------------------------------------
 ** This software is in the public domain, furnished "as is", without technical
 ** support, and with no warranty, express or implied, as to its usefulness for
 ** any purpose.
 **
 ** perf.h
 ** Contains functions for getting the values of perf counters.
 **
 ** Author: Kostas Zoumpatianos <zoumpatianos@disi.unitn.it>
 ** -------------------------------------------------------------------------*/

 ===============================================
 +                   EXAMPLE                   +
 ===============================================
 
 #include <stdlib.h>
 #include <stdio.h>
 #include <unistd.h>
 #include <string.h>
 #include <sys/ioctl.h>
 #include <linux/perf_event.h>
 #include <asm/unistd.h>
 #include "perf.h"
 
 #define EVENTC          18
 #define DOUBLE_EVENTC   (EVENTC * 2)
 #define EVENTVALC       (EVENTC + 1)
 
 int
 main(int argc, char **argv)
 {
     // Number of event counters to be monitored
     int eventc = EVENTC;
     
     // Counters to be monitored
     int eventv[DOUBLE_EVENTC] = {PERF_COUNT_HW_CPU_CYCLES,              PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_INSTRUCTIONS,             PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_CACHE_REFERENCES,         PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_CACHE_MISSES,             PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_BRANCH_INSTRUCTIONS,      PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_BRANCH_MISSES,            PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_BUS_CYCLES,               PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_STALLED_CYCLES_FRONTEND,  PERF_TYPE_HARDWARE,
                                 PERF_COUNT_HW_STALLED_CYCLES_BACKEND,   PERF_TYPE_HARDWARE,
                                 PERF_COUNT_SW_CPU_CLOCK,                PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_TASK_CLOCK,               PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_PAGE_FAULTS,              PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_CONTEXT_SWITCHES,         PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_CPU_MIGRATIONS,           PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_PAGE_FAULTS_MIN,          PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_PAGE_FAULTS_MAJ,          PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_ALIGNMENT_FAULTS,         PERF_TYPE_SOFTWARE,
                                 PERF_COUNT_SW_EMULATION_FAULTS,         PERF_TYPE_SOFTWARE
     };
     
     // Array where counts are going to be stored
     long long eventval[EVENTVALC] = {0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0};
     
     // Start measuring
     int *fd = start_measuring(eventc, eventv);
     
     // Do something
     printf("Measuring counts for this printf\n");
     
     // Stop measuring
     stop_measuring(fd, eventc, eventval);
     
     // Pring values
     print_event_counts(eventc, eventval);
 }
