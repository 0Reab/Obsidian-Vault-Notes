```python
import matplotlib.pyplot as plt  
import numpy as np  
  
  
def plot_basic(data):  
    x_date = data.keys()  
    y_amount = data.values()  
  
    fig, ax = plt.subplots()  
  
    legend_labels = {  
        'tab:red': 'Računi',  
        'tab:orange': 'Hrana',  
        'tab:green': 'Oprema',  
#        'tab:blue': 'other'  
    }  
  
    handles = [  
        plt.Line2D([0], [0], color=color, lw=4, label=label)  
        for color, label in legend_labels.items()  
    ]  
    bar_colors = []  
    connect = {  
        'Računi': 'tab:red',  
        'Hrana': 'tab:orange',  
        'Oprema': 'tab:green'}  
  
    def color_map(item):  
        identifier = item.split(' ')[1]  
        try:  
            color = connect[identifier]  
        except KeyError:  
            color = 'tab:blue'  
        bar_colors.append(color)  
  
    for x in x_date:  
        color_map(x)  
  
#    bar_colors = ['tab:red' if 'Računi' in x else 'tab:blue' for x in x_date]  
  
    ax.legend(handles=handles)  
    ax.bar(x_date, y_amount, color=bar_colors)  
    plt.show()
```

This example show how to plot a graph of two iterables.
Have them be bars (pillars) in the graph, colored by the category of item in iterable.
Also, color legend is present.