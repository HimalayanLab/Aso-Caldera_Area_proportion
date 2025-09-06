# Aso-Caldera_Area_proportion
The script was made to categorized the area proportion/distribution in terms of the direction facing.
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import LinearSegmentedColormap

# Full data
aspect_labels_full = [
    "Flat (-1)", "North (0-22.5)", "Northeast (22.5-67.5)", "East (67.5-112.5)",
    "Southeast (112.5-157.5)", "South (157.5-202.5)", "Southwest (202.5-247.5)",
    "West (247.5-292.5)", "Northwest (292.5-337.5)", "North (337.5-360)"
]
fr_values_full = [1.269138, 7.078141, 11.277535, 9.744151, 11.382517, 13.124825, 13.656895, 12.384176, 13.444506, 6.638117]

# Select main directions only for axis labels and legend
main_dirs = ["N", "NE", "E", "SE", "S", "SW", "W", "NW"]
main_indices = [1, 2, 3, 4, 5, 6, 7, 8]  # indices for above from full list

# Corresponding FR values and colors for main directions
fr_values = [fr_values_full[i] for i in main_indices]

# Color ramp (yellow to blue)
colors = ["#FFD700", "#ADFF2F", "#00FFCC", "#1E90FF", "#000080"]
cmap = LinearSegmentedColormap.from_list("aspect_cmap", colors, N=len(main_dirs))
aspect_colors = [cmap(i/(len(main_dirs)-1)) for i in range(len(main_dirs))]

def directional_pie_with_legend(labels, values, colors, filename=None, dpi=600, font_size=18):
    N = len(values)
    theta = np.linspace(0.0, 2 * np.pi, N, endpoint=False)
    width = 2 * np.pi / N

    fig, ax = plt.subplots(figsize=(8,8), subplot_kw={'projection': 'polar'})
    bars = ax.bar(theta, values, width=width*0.95, color=colors, edgecolor='black', align='edge')

    # Configure axis
    ax.set_yticklabels([])
    ax.set_xticks(theta)
    ax.set_xticklabels(labels, fontsize=18, fontweight='bold')

    # Draw circular boundary and grid
    ax.spines['polar'].set_visible(True)
    ax.spines['polar'].set_linewidth(1.5)
    ax.grid(True, linestyle='--', alpha=0.5)


    # Add legend below chart with directions and FR values
    legend_labels = [f"{lbl}: {val:.2f}" for lbl, val in zip(labels, values)]
    legend = ax.legend(bars, legend_labels, loc='upper center', bbox_to_anchor=(0.5, -0.1),
                fontsize=18, frameon=False, ncol=4, title="Aspect values")
    legend.get_title().set_fontweight('bold')

    plt.tight_layout()

    # Save if filename specified
    if filename:
        plt.savefig(filename, format='tiff', dpi=dpi, bbox_inches='tight')

    plt.show()

# Run the plot with main directions, values, and colors
directional_pie_with_legend(main_dirs, fr_values, aspect_colors, filename='aspect_dir_pie.tif', font_size=14)
