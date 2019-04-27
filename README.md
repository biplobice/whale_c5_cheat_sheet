# Whale Concrete5 Cheat Sheet V8+

This is a collection of Concrete5 cheat sheets, based on the C5 V8+ source code. 

**Contributions are welcome via [issues](https://github.com/shahroq/whale_c5_cheat_sheet/issues) and [pull requests](https://github.com/shahroq/whale_c5_cheat_sheet/pulls).**

## Table of Contents
* [Pages (Collections)](#pages)
* [Files](#files)
* [Users](#users)
* [Single Pages](#single-pages)
* [Blocks](#blocks)
* [Stacks](#stacks)
* [Language](#language)
* [Constants](#constants)
* [Configs](#configs)
* [Helpers](#helpers)
* [Contributors](#contributors)

## Pages (Collections)

### A page

#### Get Current page
```PHP
$page = \Page::getCurrentPage();
```
#### Get a page by a unique identifier
```PHP
$page = \Page::getByID(1); //by ID
$page = \Page::getByPath('/path/to/page'); //by path
```
#### Get a page data
```PHP
echo $page->getCollectionName();
echo $page->getCollectionDescription();
echo $page->getCollectionDateAdded();
echo $page->getCollectionDatePublic();
echo $page->getCollectionUserID();
```

#### Get a page type/template
```PHP
//Page Template
$pt = $page->getPageTemplateObject();
if (is_object($pt)) {
    $ptID = $pt->getPageTemplateID(); //echo $ptID;
    $ptName = $pt->getPageTemplateName(); //echo $ptName;
    $ptHandle = $pt->getPageTemplateHandle(); //echo $ptHandle;
}        

//Page Type (not working)
$pt = $page->getPageTypeObject();
if (is_object($pt)) {
    $ptID = $pt->getPageTypeID(); //echo $ptID;
    $ptName = $pt->getPageTypeName(); //echo $ptName;
    $ptHandle = $pt->getPageTypeHandle(); //echo $ptHandle;
}        
```

#### Get page attributes
```PHP
//attribute
$attr = $page->getAttribute('attribute_handle');

//Option List: get options
foreach ((object)$attr as $option) {
    $optionID = $option->getSelectAttributeOptionID(); //echo $optionID;
    $optionValue = $option->getSelectAttributeOptionValue(); //echo $optionValue;
}

//Image/File
if ($attr) {
    $attrURL = $attr->getURL(); //echo $attrURL);
    $attrDownloadURL = $attr->getDownloadURL(); //echo $attrDownloadURL;
    $attrForceDownloadURL = $attr->getForceDownloadURL(); //echo $attrForceDownloadURL;
    $attrSize = $attr->getSize(); //echo $attrSize;
    $attrExtension = $attr->getExtension(); //echo $attrExtension;

    //image thumbnail
    $im = Core::make('helper/image');
    $thumbSrc = $im->getThumbnail($attr, 100, 100)->src; //echo $thumbSrc;
}
```

#### Get list of attributes of a page
```PHP

```



### List of pages



## Files
### A Files
#### Get a file by a unique identifier
```PHP
$file = \File::getByID(1); // by ID
```

#### Get a file data
```PHP
echo $file->getFileID();
echo $file->getURL();

//file version
$fileVersion = $file->getApprovedVersion();
```
#### Get a file attributes
```PHP

```
#### Get list of attributes of a file
```PHP

```


## Users

#### Get a user by a unique identifier
```PHP
$user = \File::getByID(1); // by ID
```

#### Get a user data
```PHP
$userGroups = $user->getUserGroups(); //print_r($userGroups);
$userIsActive = $user->isActive(); //print_r($userIsActive);
$userIsSuperUser = $user->isSuperUser(); //print_r($userIsSuperUser);
$userLastOnline = $user->getLastOnline(); //print_r(date('Y-m-d H:i:s', $userLastOnline));
$userUserName = $user->getUserName(); //print_r($userUserName);
$userIsRegistered = $user->isRegistered(); //print_r($userIsRegistered);
$userUserID = $user->getUserID(); //print_r($userUserID);
$userTimezone = $user->getUserTimezone(); //print_r($userTimezone);
```

#### Get user info object
```PHP
$ui = UserInfo::getByID(1); //by ID
$ui = UserInfo::getByUserName('admin'); //by username
$ui = UserInfo::getByEmail('jane@concrete5.org'); //by email
```

#### Get user info data
```PHP
$uiIsActive = $ui->isActive(); //echo $uiIsActive;
$uiUserName = $ui->getUserName(); //echo $uiUserName;
$uiEmail = $ui->getUserEmail(); //echo $uiEmail;
$uiNumLogins = $ui->getNumLogins(); //echo $uiNumLogins;
$uiLastIPAddress = $ui->getLastIPAddress(); //echo $uiLastIPAddress;
$uiLastLogin = $ui->getLastLogin(); //echo date('Y-m-d H:i:s', $uiLastLogin);
$uiPreviousLogin = $ui->getPreviousLogin(); //echo date('Y-m-d H:i:s', $uiPreviousLogin);
$uiPublicProfileUrl = $ui->getUserPublicProfileUrl(); //echo $uiPublicProfileUrl;
$uiHasAvatar = $ui->hasAvatar(); //echo $uiHasAvatar;
$uiAvatar = $ui->getUserAvatar(); //echo $uiAvatar->getPath();
```

## Attributes

#### List of attribute set categories (collection/user/file/site/event)
```PHP
//use Concrete\Core\Attribute\Key\Category as AttributeKeyCategory;
$categories = AttributeKeyCategory::getList();		
foreach ((array)$categories as $category) {
	$categoryID = $category->getAttributeKeyCategoryID(); //echo $categoryID;
	$categoryHandle = $category->getAttributeKeyCategoryHandle(); //echo $categoryHandle;
}
```

#### List of sets in a category
```PHP
//use Concrete\Core\Attribute\Key\Category as AttributeKeyCategory;
$categoryID = 1; 
$category = AttributeKeyCategory::getByID($categoryID);
if (is_object($category)) {
    $sets = $category->getController()->getSetManager()->getAttributeSets();
    foreach ((object)$sets as $set) {
        $setID = $set->getAttributeSetID(); //echo $setID);
        $setHandle = $set->getAttributeSetHandle(); //echo $setHandle;
        $setName = $set->getAttributeSetDisplayName(); //echo $setName;
	}
}	
```

#### Get a Collection/Page attribute
```PHP
$attr = CollectionAttributeKey::getByID(1); //by ID
$attr = CollectionAttributeKey::getByHandle('attribute_handle'); //by handle

$attrID = $attr->getAttributeKeyID(); //echo $attrID;
$attrHandle = $attr->getAttributeKeyHandle(); //echo $attrHandle;
$attrName = $attr->getAttributeKeyName(); //echo $attrName;
```
#### Get options of an 'Option List' attribute
```PHP
$attr = CollectionAttributeKey::getByHandle('attribute_handle'); //by handle
$controller = $attr->getController();
$attrOptions = $controller->getOptions();
foreach ((object)$attrOptions as $attrOption) {
    $attrOptionID = $attrOption->getSelectAttributeOptionID(); //echo $attrOptionID;
    $attrOptionValue = $attrOption->getSelectAttributeOptionValue(); //echo $attrOptionValue;
}
```

### Attrubute Set

#### A set
```PHP
$attrSet = AttributeSet::getByID('attribute_set_id'); //by ID
$attrSet = AttributeSet::getByHandle('attribute_set_handle'); //by handle
```
#### Attributes in a set
```PHP
$attrs = $attrSet->getAttributeKeys();
foreach($attrs as $attr) {
    $attrID = $attr->getAttributeKeyID(); //echo $attrID;
    $attrHandle = $attr->getAttributeKeyHandle(); //echo $attrHandle;
    $attrName = $attr->getAttributeKeyName(); //echo $attrName;
}
```

#### List of sets
```PHP

```


## Single Pages


## Blocks


## Stacks


## Language


## Constants


## Configs


## Helpers

#### Number helper
```PHP
$im = Core::make('helper/number');
```

#### Text helper
```PHP
$im = Core::make('helper/text');
```

#### URL helper
```PHP
$im = Core::make('helper/url');
```

#### Image helper
```PHP
$im = Core::make('helper/image');
```


## Contributors
