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
ORDER BY SalePrice DESC

Output:
![image](https://github.com/kelachicb/DataCleaning/assets/57774879/9c48c255-8dcb-499f-9c50-42e2294a30e8)

Looking at the data, we can see some of the information is repeated. We are going to check for any duplicates within our data. And if any we will remove.

We are checking for Duplicates using a CTE (common Table Expression)
WITH RowNumCTE AS(
SELECT 
	UniqueID, ParcelID, PropertyAddress, LandValue,
	BuildingValue, TotalValue, YearBuilt, LandUse,
	Bedrooms, FullBath, HalfBath, SaleDate, SalePrice,
	SoldAsVacant, LegalReference,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID,
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) Row_num
FROM NashvilleHousingData
)
SELECT *
FROM RowNumCTE
Where Row_num > 1
ORDER BY SalePrice

Output:
![image](https://github.com/kelachicb/DataCleaning/assets/57774879/9de1b574-d98a-44d3-8da9-fc7e15eaed1e)

From our output, we can see multiple duplicates in our data; We would like to remove those duplicates. We will do that by using the `DELETE` 

WITH RowNumCTE AS(
SELECT 
	UniqueID, ParcelID, PropertyAddress, LandValue,
	BuildingValue, TotalValue, YearBuilt, LandUse,
	Bedrooms, FullBath, HalfBath, SaleDate, SalePrice,
	SoldAsVacant, LegalReference,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID,
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) Row_num
FROM NashvilleHousingData
)
DELETE
FROM RowNumCTE
Where Row_num > 1

Output:
![image](https://github.com/kelachicb/DataCleaning/assets/57774879/fe08acf8-2193-4002-8f74-cfb4660ff8e7)

Our output confirms that 103 duplicate records have been removed. Now, let's repeat to confirm our duplicates have been removed.

WITH RowNumCTE AS(
SELECT 
	UniqueID, ParcelID, PropertyAddress, LandValue,
	BuildingValue, TotalValue, YearBuilt, LandUse,
	Bedrooms, FullBath, HalfBath, SaleDate, SalePrice,
	SoldAsVacant, LegalReference,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID,
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) Row_num
FROM NashvilleHousingData
)
SELECT *
FROM RowNumCTE
Where Row_num > 1
ORDER BY SalePrice

Output:
![image](https://github.com/kelachicb/DataCleaning/assets/57774879/2157c3ff-b071-49b5-8eea-1ca32f289896)

We can see no data has been returned, which confirms all duplicates in our data have been removed.



