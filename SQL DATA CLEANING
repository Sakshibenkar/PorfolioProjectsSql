/*

Cleaning data in sql

*/

select * 
from dbo.NashvilleHousing


----------------------------------------------------------------------------


--standardize date format

select SaleDateConverted, CONVERT(Date,SaleDate)
from dbo.NashvilleHousing

Update NashvilleHousing
SET SALEDATE = CONVERT(Date,SaleDate)

ALTER TABLE NashvilleHousing
ADD SaleDateConverted date;

Update NashvilleHousing
SET SaleDateConverted = CONVERT(Date,SaleDate)


----------------------------------------------------------------------------


--Populate PropertyAddress data

select *
from dbo.NashvilleHousing
--WHERE PropertyAddress is null
order by ParcelID

select a.ParcelID,a.PropertyAddress,b.ParcelID,b.PropertyAddress, isnull(a.PropertyAddress,b.PropertyAddress)
from dbo.NashvilleHousing a
join dbo.NashvilleHousing b
on a.ParcelID = b.ParcelID
and a.[UniqueID ]<> b.[UniqueID ]
where a.PropertyAddress is null

update a
set propertyaddress = isnull(a.PropertyAddress,b.PropertyAddress)
from dbo.NashvilleHousing a
join dbo.NashvilleHousing b
on a.ParcelID = b.ParcelID
and a.[UniqueID ]<> b.[UniqueID ]
where a.PropertyAddress is null

--Breaking out Adress into individual columns (Adress,City,State)

select propertyaddress
from dbo.NashvilleHousing 

SELECT
SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 ) as Address
, SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress)) as Address

From PortfolioProject.dbo.NashvilleHousing



ALTER TABLE NashvilleHousing
ADD PropertySplitAddress Nvarchar(255);

Update NashvilleHousing
SET PropertySplitAddress  = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 ) 


ALTER TABLE NashvilleHousing
ADD PropertySplitcity  Nvarchar(255);

Update NashvilleHousing
SET PropertySplitcity= SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress))


SELECT *
From dbo.NashvilleHousing


select ownerAddress
from PortfolioProject.dbo.NashvilleHousing


select 
PARSENAME(REPLACE(OwnerAddress,',' , '.'), 3),
PARSENAME(REPLACE(OwnerAddress,',' , '.'), 2),
PARSENAME(REPLACE(OwnerAddress,',' , '.'), 1)
from PortfolioProject.dbo.NashvilleHousing


ALTER TABLE NashvilleHousing
ADD ownerSplitAddress Nvarchar(255);

Update NashvilleHousing
SET ownerSplitAddress= PARSENAME(REPLACE(OwnerAddress,',' , '.'), 3) 

ALTER TABLE NashvilleHousing
ADD ownerSplitcity Nvarchar(255);

Update NashvilleHousing
SET ownerSplitcity= PARSENAME(REPLACE(OwnerAddress,',' , '.'), 2)

ALTER TABLE NashvilleHousing
ADD PropertySplitState Nvarchar(255);

Update NashvilleHousing
SET PropertySplitState=PARSENAME(REPLACE(OwnerAddress,',' , '.'), 1)


select *
from PortfolioProject.dbo.NashvilleHousing


----------------------------------------------------------------------------


-- Change Y and N to Yes and No in "Sold as Vacant" field


Select Distinct(SoldAsVacant), Count(SoldAsVacant)
From PortfolioProject.dbo.NashvilleHousing
Group by SoldAsVacant
order by 2

Select SoldAsVacant
,Case when SoldAsVacant = 'y' then 'Yes'
     when SoldAsVacant = 'n' then 'No'
	 else SoldAsVacant
	 end
From PortfolioProject.dbo.NashvilleHousing


Update NashvilleHousing
SET  SoldAsVacant =Case when SoldAsVacant = 'y' then 'Yes'
     when SoldAsVacant = 'n' then 'No'
	 else SoldAsVacant
	 end


----------------------------------------------------------------------------

-- Remove Duplicates

WITH RowNumCTE AS(
Select *,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID,
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) row_num

From PortfolioProject.dbo.NashvilleHousing
--order by ParcelID
)
select*
From RowNumCTE
Where row_num > 1
--Order by PropertyAddress

Select *
From PortfolioProject.dbo.NashvilleHousing


----------------------------------------------------------------------------

-- Delete Unused Columns



Select *
From PortfolioProject.dbo.NashvilleHousing


ALTER TABLE PortfolioProject.dbo.NashvilleHousing
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress, SaleDate

