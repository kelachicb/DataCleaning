# DataCleaning

## Project Description
The Goal of this project is to clean-up Data for easy querying

## Data Loading

## Lets Dive in
Now our Data has been loaded, we start by selecting all our data to locate which columns need to be cleaned up for easier querying


SELECT 
	UniqueID, ParcelID, PropertyAddress, LandValue,
	BuildingValue, TotalValue, YearBuilt, LandUse,
	Bedrooms, FullBath, HalfBath, SaleDate, SalePrice,
	SoldAsVacant
FROM NashvilleHousingData
Output:
![image](https://github.com/kelachicb/DataCleaning/assets/57774879/e0befed3-868d-4d7a-a2d3-e76c6dcd5c77)

