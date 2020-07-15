from matplotlib.pyplot import *
from matplotlib.dates import DateFormatter, DAILY, WEEKLY, MO, rrulewrapper, RRuleLocator
import datetime as dt
import numpy as np

today = dt.datetime.today().toordinal()
# view range or view box
time_range_to_view = (dt.datetime.strptime('25/06/2020', '%d/%m/%Y').toordinal(),dt.datetime.strptime('30/07/2020', '%d/%m/%Y').toordinal())
# project start date
project_start_date_ord = dt.datetime.strptime('01/7/2020', '%d/%m/%Y').toordinal()

tasksd = {
    # start days after project start, deadline-date, completed %, color-of-bar, bg-color-text
    'Task 1':   (2, '24/07/2020', 0, '#dddddd','yellow'),
    'Task 2':   (2, '16/07/2020', 0, '#bb9922','white'),
    'Task 3':   (0, '04/09/2020', 0, '#ff6644' ,'white'),
    'Task 4':   (0, '09/09/2020', 0, 'blue'   ,'white'),
    'Task 5':   (0, '29/09/2020', 0, 'green'  ,'white'),
          }
rcParams['font.size'] = 18.0
day_tick_frequency = 5
rot_xticks = 45
figsize = (14,2.9)
space_top = 0.8
space_below = -0.8
bar_height = 1.0
bar_sep = 0.2

def from_start_duration_lst(tasklist):
    dates = []
    tasknms = []
    progress = []
    tcolor = []
    for k,d in zip(tasklist.keys(), tasklist.values()):
        start = project_start_date_ord + d[0]
        stop = dt.datetime.strptime(d[1], '%d/%m/%Y').toordinal()
        duration = stop-start
        dates.append((start, duration))
        progress.append((start, duration*d[2]/100))
        tasknms.append(k)
        if len(d)>3:
          tcolor.append(d[3])
        else:
            tcolor.append('g')
    return dates, tasknms, progress, tcolor

dates, tasknms, progress, tcolor = from_start_duration_lst( tasksd )

# dfmt = DateFormatter('%d-%b')
dfmt = DateFormatter('%d/%m')

fig = figure(figsize=figsize)

ax = fig.add_subplot(111)

task_y_deltas = np.arange(len(dates))
task_y_pos = []

for dy,date,p,c in zip(task_y_deltas,dates,progress,tcolor):
    y_pos = -dy - dy*bar_sep
    task_y_pos.append( y_pos + bar_height/2 )
    ax.broken_barh(xranges=[date], yrange=( y_pos, bar_height),color=c,alpha=0.5,zorder=1.9)
    #progress
    ax.broken_barh(xranges=[p], yrange=( y_pos, bar_height),color='b',alpha=0.5,zorder=2.0)

yticks(task_y_pos,tasknms)
for lbl,tn in zip(ax.get_yticklabels(),tasknms):
  # set ylable background colors
  lbl.set_backgroundcolor(tasksd[lbl.get_text()][4])
# highlight end points with a cross
# ax.plot(post_doc_dl,task_y_pos[0],'x',ms=15,markeredgewidth=5,color='r')
# ax.plot(OSA_dl,task_y_pos[1],'x',ms=15,markeredgewidth=5,color='r')

ax.xaxis.set_major_locator(RRuleLocator(rrulewrapper(DAILY,interval=day_tick_frequency)))
ax.xaxis.set_major_formatter(dfmt)
xticks(rotation=rot_xticks)
# today line
ax.plot([today]*2,[min(task_y_pos)+space_below,max(task_y_pos)+space_top],linewidth=5,color='r')
ylim((min(task_y_pos)+space_below,max(task_y_pos)+space_top))
### xlim should be used as otherwise the scaling does not match the data points
xlim(time_range_to_view)
grid(axis='x')
tight_layout()
show()
