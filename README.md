# Packing
To run on your system please start a [virtual environment](https://docs.python.org/3/library/venv.html)
then run `pip install -r requirements.txt` in your shell. 

# Problem Statement
I want to help **owners** organize packing items.
The will be grouped into **containers**, **categories**, and **subcategories** with detailed information about each **item**.

# Schema 
## Entities
* Profiles -- e.g. jakobloverde@proton.me - _One to One_ with Owners
* Owners -- e.g. Jakob - _One to Many_
* Containers -- e.g. Suitcase, backpack, carry-on - _One to many_ with (many) categories
* Categories -- e.g. Technology, Clothes, Travel Documents - _One to many_ with (many) subcategories 
* Subcategories -- e.g. Pants, Shirts, underwear - _One to many_ with (many) items
* Items -- e.g. Black Jeans Regular Fit, Passport, Laptop - _Many to Many_ 1 item can belong to multiple owners

## Attribute Sets
_each entity E and its atribute set V_
* Profiles :: (profile_id, owner_id, bio)
* Owners :: (name, owner_id)
* Containers :: (container_id, container_type, owner_id, weight_limit)
* Categories :: (category_name, container_id)
* Subcategories :: (subcategory_name, category_name)
* Items :: (item_id, description, subcategory, owner_ids)

## Functional Dependencies
_A -> B exist if, for every value of A, there is EXACTLY one corresponding B_
### Profiles
* profile_id -> {owner_id, bio} 
* owner_id -> {profile_id}
### Owners
* owner_id -> {name}
### Containers
* container_id -> {container_type, owner, weight_limit}
* container_id -> {owner_id}
### Categories
* category -> {container_id}
### Subcategories
* subcategory_name -> {category_name}
### Items
* item_id -> {description, subcategory_name}

## Relationships
1. Profiles <-> Owners (One-to-One)
   ∀p∈Profiles,∃!o∈Owners : Profiles.owner_id = Owners.owner_id
   _For every profile p in the set of Profiles, there exists exactly one owner o in the set of Owners where Profiles.owner_id == Owners.owner_id._

2. Owners -> Containers (One-to-Many)
   ∀o∈Owners,∃c∈Containers : Containers.owner_id = Owners.owner_id
   _For every owner o in the set of Owners, there exists one or more containers c in the set of Containers such that Containers.owner_id == Owners.owner_id._

3. Containers -> Categories (One-to-Many)
   ∀c∈Containers,∃d∈Categories : Categories.container_id = Containers.container_id
   _For every container c in the set of Containers, there exists one or more categories d in the set of Categories such that Categories.container_id == Containers.container_id._

4. Categories -> Subcategories (One-to-Many)
   ∀c∈Categories,∃s∈Subcategories : Subcategories.category_name = Categories.category_name
   _For every category c in the set of Categories, there exists one or more subcategories s in the set of Subcategories such that Subcategories.category_name == Categories.category_name._

5. Subcategories -> Items (One-to-Many)
   ∀s∈Subcategories,∃i∈Items : Items.subcategory_name = Subcategories.subcategory_name
   _For every subcategory s in the set of Subcategories, there exists one or more items i in the set of Items such that Items.subcategory_name == Subcategories.subcategory_name._

6. Items <-> Owners (Many-to-Many)
   ∀i∈Items,∃o∈Owners : (owner_id, item_id) ∈ Owner_Items where Owner_Items ⊆ Owners.owner_id × Items.item_id
   _For every item i in the set of Items, there exists one or more owners o in the set of Owners such that (owner_id, item_id) ∈ Owner_Items, where Owner_Items is a subset of all possible pairs of Owners.owner_id and Items.item_id._
