<p align="center">
  <img src="https://github.com/IgorZaton/Data-Science-Tips/assets/50952749/6074bc6a-9749-4cee-83d8-13bf59f1b2dc" width="350" height="350"/>
</p>

### 1. Use dataclasses for results storage if result format is important.

If you’re designing some kind of algorithm that returns many things that are used separately afterwards, think about using dataclass to define this result format.

Example:

Without dataclass:
```python
# return format
        return mm_per_pixel, remaining_blisters_mask, all_blisters_mask, plate_image, excluded_area_mask

# Usage:
result = algorithm(alg_input)
metric = Metric(result[:2])
visualizer = Visualizer(result[1:])
…
```

With dataclass:
```python

# Dataclass
@dataclass
class ImageProcessingAlgorithmResult:
    """
    Dataclass of result of image processing.

    Attributes:
        metric_artifacts: artifacts of the metric
        visualisation_artifacts: artifact used in visualisation
    """

    metric_artifacts: MetricArtifacts
    visualisation_artifacts: VisualisationArtifacts

# return format
        return ImageProcessingAlgorithmResult(
            metric_artifacts=MetricArtifacts(
                mm_per_pixel=mm_per_pixel,
                artifacts=(remaining_blisters_mask, all_blisters_mask),
            ),
            visualisation_artifacts=VisualisationArtifacts(
                image=plate_image,
                artifacts=(
                    remaining_blisters_mask,
                    all_blisters_mask,
                    excluded_area_mask,
                ),
            ),
        )

# Usage:
result = algorithm(alg_input)
metric = Metric(result.metric_artifacts)
visualizer = Visualizer(result.visualisation_artifacts)
…
```

If format of specific field in dataclass is important as well, you can make another dataclass to store it.

It’s much more readable (you’re not using indexes like result[2], result[420], which are pretty enigmatic) and provides you more control over the whole pipeline format.

### 2. Use poetry as your package manager

There’re many package managers out there for you to use. You can use conda, pyenv or install packages globally with pip (god please no). From my experience the best solution for package/environment management is to use poetry.

I think poetry is the best one I’ve worked with so far because:
- It’s easy to start with, you can install it with pip using ```pip install poetry``` and ```poetry init``` to init your environment.
- It’s easy to use, you can install a new package with ```poetry add package_name``` and remove a package with ```poetry remove package_name```.
- It doesn’t have many distributions for packages like conda does.
- It doesn’t give me any errors, or at least not as much as conda when I use it. If you’re keeping things clean, you’ll probably not have any problems with it.
- It’s easy to share a poetry environment across the team - when you just need to share .lock and .toml file. When you receive .lock and .toml you can install an environment with ```poetry install``` and you’re good to go.

It’s possible there’s some other, better tool for package and dependency management, but what I like in poetry is simplicity and the fact that even an inexperienced user can start a project with it within a few minutes.

### 3. Solve poetry.lock conflicts with regeneration of the file.

If you’re working in a team, it’s possible you’ll bump into some conflicts regarding the environment. Solving it might be a little bit tricky, especially the .lock file case as it might be even a few thousand lines long.

To make sure you’ve solved conflicts properly, and didn’t break the environment during the process follow these steps:
1. Remove poetry.lock file.
2. Solve conflicts in the pyproject.toml file.
3. Use ```poetry lock``` command to recreate the updated lock file.

First step might seem unnecessary, but it’s good to have it as a double check that everything goes right.

