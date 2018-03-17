import codecademylib
import pandas as pd
import matplotlib.pyplot as plt
species = pd.read_csv('species_info.csv')
print species.head()
species_count = species.scientific_name.nunique()
print(species_count)
species_type = species.category.unique()
print species_type
conservation_statuses = species.conservation_status.unique()
print conservation_statuses
conservation_counts = species.groupby('conservation_status').scientific_name.nunique().reset_index()
print conservation_counts
species.fillna('No Intervention', inplace = True)
conservation_counts_fixed = species.groupby('conservation_status').scientific_name.nunique().reset_index()
print conservation_counts_fixed

---------------------------------------

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)
protection_counts = species.groupby('conservation_status')\
    .scientific_name.nunique().reset_index()\
    .sort_values(by='scientific_name')
#1 Start by creating a wide figure with figsize = (10,4)
plt.figure(figsize=(10, 4))
#2 Create an axes objected called ax using plt.subplot
ax = plt.subplot()
#3 Create a bar chart whose heights are equal to the scientific_name column of protection_counts.
plt.bar(range(len(protection_counts)),protection_counts.scientific_name.values)
#4 Create an x-tick for each of the bars.
ax.set_xticks(range(len(protection_counts)))
#5 Label each x-tick with the label from conservation_status in protection_counts.
ax.set_xticklabels(['In Recovery', 'Threatened',
'Endangered','Species of Concern','No Intervention'])
#6 Label the y-axis Number of Species.
plt.ylabel('Number of Species')
#7 Title the graph Conservation Status by Species
plt.title('Conservation Status by Species')
#8 Plot the graph using plt.show().
plt.show()

---------------------------------------

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)

species['is_protected'] = species.conservation_status != 'No Intervention'

category_counts = species.groupby(['category', 'is_protected']).scientific_name.nunique().reset_index()
category_pivot = category_counts.pivot(columns='is_protected',
                      index='category',
                      values='scientific_name')\
                      .reset_index()
category_pivot.columns = ['category','not_protected','protected']
category_pivot['percent_protected'] = category_pivot.protected / (category_pivot.protected + category_pivot.not_protected)
print category_pivot

---------------------------------------

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt
from scipy.stats import chi2_contingency
# 1 mammanl vs bird
contingency = [[30,146],[75,413]]
pval = chi2_contingency(contingency)[1]
print (pval)
# 2 mammal vs reptile
contingency_reptile_mammal = [[30,146],[5,73]]
pval_reptile_mammal = chi2_contingency(contingency_reptile_mammal)[1]
print (pval_reptile_mammal)
# 3 Vascular Plant	vs Nonvascular plant
contingency_vasplant_nonvasplant = [[46,4216],[5,328]]
pval_vasplant_nonvasplant = chi2_contingency(contingency_vasplant_nonvasplant)[1]
print (pval_vasplant_nonvasplant)
# 4 Fish	vs Amphibian
contingency_fish_amphibian = [[11,115],[7,72]]
pval_fish_amphibian = chi2_contingency(contingency_fish_amphibian)[1]
print (pval_fish_amphibian)
# 5 Bird vs Reptile
contingency_bird_reptile = [[75,413],[5,73]]
pval_bird_reptile = chi2_contingency(contingency_bird_reptile)[1]
print (pval_bird_reptile)
# 6 Vascular Plant	vs Bird
contingency_vasplant_bird = [[46,4216],[75,413]]
pval_vasplant_bird = chi2_contingency(contingency_vasplant_bird)[1]
print (pval_vasplant_bird)
# 7 Amphibian_Bird	
contingency_amphibian_bird = [[7,72],[75,413]]
pval_amphibian_bird = chi2_contingency (contingency_amphibian_bird) [1]	
print (pval_amphibian_bird)
#8 Amphibian_Mammal
contingency_Amphibian_Mammal =[[7,72],[30,146]]
pval_Amphibian_Mammal = chi2_contingency(contingency_Amphibian_Mammal)[1]
print(pval_Amphibian_Mammal)
#9 Amphibian_Nonvascular plant
contingency_Amphibian_Nonvascularplant =[[7,72],[5,328]]
pval_Amphibian_Nonvascularplant= chi2_contingency(contingency_Amphibian_Nonvascularplant)[1]
print(pval_Amphibian_Nonvascularplant)
#10 Amphibian_Reptile
contingency_Amphibian_Reptile = [[7,72],[5,73]]
pval_Amphibian_Reptile = chi2_contingency(contingency_Amphibian_Reptile)[1]
print(pval_Amphibian_Reptile)
#11 Amphibian_Vascularplant
contingency_Amphibian_Vascularplant = [[7,72],[46,4216]]
pval_Amphibian_Vascularplant=chi2_contingency(contingency_Amphibian_Vascularplant)[1]
print(pval_Amphibian_Vascularplant)
#12 Bird_Nonvascularplant
contingency_Bird_Nonvascularplant = [[75,413],[5,328]]
pval_Bird_Nonvascularplant=chi2_contingency(contingency_Bird_Nonvascularplant)[1]
print(pval_Bird_Nonvascularplant)
#13 Mammal_Nonvascularplant
contingency_Mammal_Nonvascularplant = [[30,146],[5,328]]
pval_Mammal_Nonvascularplant=chi2_contingency(contingency_Mammal_Nonvascularplant)[1]
print(pval_Mammal_Nonvascularplant)
#14 Fish_Reptile
contingency_Fish_Reptile = [[11,115],[5,73]]
pval_Fish_Reptile = chi2_contingency(contingency_Fish_Reptile)[1]
print(pval_Fish_Reptile)


---------------------------------------

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species.fillna('No Intervention', inplace = True)
species['is_protected'] = species.conservation_status != 'No Intervention'

observations = pd.read_csv('observations.csv')
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
species_is_sheep = species[species.is_sheep]
print species_is_sheep.head()
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]
print sheep_species.head()
sheep_observations = pd.merge(sheep_species, observations)
print sheep_observations.head()
obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()
print obs_by_park

---------------------------------------

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]

observations = pd.read_csv('observations.csv')

sheep_observations = observations.merge(sheep_species)

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()

#1 Start by creating a wide figure with figsize = (16,4)
plt.figure(figsize=(16, 4))
#2 Create an axes objected called ax using plt.subplot
ax = plt.subplot()
#3 Create a bar chart whose heights are equal to observations column of protection_counts.
plt.bar(range(len(obs_by_park)),obs_by_park.observations.values)
#4 Create an x-tick for each of the bars.
ax.set_xticks(range(len(obs_by_park)))
#5 Label each x-tick with the label from park_name in obs_by_park
ax.set_xticklabels(obs_by_park.park_name.values)
#6 Label the y-axis Number of Species.
plt.ylabel('Number of Observations')
#7 Title the graph Conservation Status by Species
plt.title('Observations of Sheep per Week')
#8 Plot the graph using plt.show().
plt.show()

---------------------------------------

baseline = 15

minimum_detectable_effect = 100*5/15

sample_size_per_variant = 510

yellowstone_weeks_observing = sample_size_per_variant/507

bryce_weeks_observing = sample_size_per_variant/250