# NAME

        Text::FixedWidth::Parser 

# DESCRIPTION

        The Text::FixedWidth::Parser module allows you to read fixed width text file by specifying string mapper

# SYNOPSIS

     use Text::FixedWidth::Parser;

     FileData
     ~~~~~~~~
     ADDRESS001XXXXX YYYYYYY84 SOUTH STREET USA
     MARK00182869890         
     ADDRESS002YYYYYYY      69 BELL STREET  UK 
     MARK00288698939         

     my $string_mapper = [
           {
               Rule => {
                   LinePrefix => [1, 7],
                   Expression => "LinePrefix eq 'ADDRESS'"
               },
               Id   => [8,  3],
               Name => [11, 13],
               Address => {DoorNo => [24, 2], Street => [26, 14]},
               Country => [40, 3]
           },
           {
               Rule => {
                   LinePrefix => [1, 4],
                   Expression => "LinePrefix eq 'MARK'"
               },
               Id    => [5,  3],
               Mark1 => [8,  2],
               Mark2 => [10, 2],
               Mark3 => [12, 2],
               Mark4 => [14, 3],
           }
     ];
    

     # StringMapper should be passed while creating object
     my $obj = Text::FixedWidth::Parser->new( 
                   { 
                      #Required Params
                      StringMapper  => $string_mapper,
                      #optional Params
                      ConcateString => '', 
                      EmptyAsUndef  => 1
                   }
               );

     open my $fh, '<', 'filename';

     $data = $obj->read($fh);

# PARAMS 

## StringMapper

    * StringMapper can be HASHRef or multiple StringMappers as ARRAY of HASHRefs.
    * If Multiple StringMappers exist, Based on Rule apropriate StringMapper will get selected.
    * In Multiple StringMappers, Its better to place Rule-less mapper after Rule based mappers.
         * Rule-less mapper will picked as soon as its get access in an array
    * StringMapper fields should be defined as ARRAY, First element as StringPoint of string and Second element as length of the string.
    * Rule, Expression are keywords, overriding or changing those will affect the functionality.

## ConcateString

    * StringMapper can have field values can be defined as {Address => [24, 2, 26, 14]}, 
    * This reperesents Address field will value will be concatenation of two strings, 
      which are has Startingpoint 24, Length 2 and Startingpoint 26, Lenght 14.
    * While concatenate those two strings value of ConcateString String will be used in between those .
          Eg: ConcateString = '-';  
          The Value of Address = 84-SOUTH STREET    
    * Space(' ') is default ConcateString

## EmptyAsUndef

    * If this flag is enabled, Empty values will be assigned as undef
        * Eg: Name = '', it will be assigned as Name = undef

# METHODS

- **get\_string\_mapper**

    Desc   : This method will return the StringMapper

    Params : NONE

    Returns: HASHRef as Mentioned in the config

- **set\_string\_mapper**

    Desc   : This method is used set the StringMapper

    Params : StringMapper

    Returns: NONE

- **get\_concate\_string**

    Desc   : This method will return the ConcateString

    Params : NONE

    Returns: ConcateString

- **set\_concate\_string**

    Desc   : This method is used to set ConcateString

    Params : String

    Returns: NONE

- **is\_empty\_undef**

    Desc   : This method will indeicate is empty flag enabled or disabled

    Params : NONE

    Returns: 1 on enabled, 0 on disabled

- **set\_empty\_undef**

    Desc   : This method used to enable or disable EmptyAsUndef flag

    Params : 1 to enable, 0 to disable

    Returns: NONE

- **read**

    Desc   : This method is used to read the line by line values

    Params : FileHandle

    Returns: HASHRef as Mentioned in the StringMapper

             Eg : {
                      'Address' => {
                        'DoorNo' => '84',
                        'Street' => 'SOUTH STREET'
                      },
                      'Country' => 'USA',
                      'Id' => '001',
                      'Name' => 'XXXXX YYYYYYY'
                   }

- **read\_all**

    Desc   : This method is used to read complete file

    Params : FileHandle

    Returns: HASHRef as Mentioned in the StringMapper

             Eg : [
                     {
                       'Address' => {
                         'DoorNo' => '84',
                         'Street' => 'SOUTH STREET'
                       },
                       'Country' => 'USA',
                       'Id' => '001',
                       'Name' => 'XXXXX YYYYYYY'
                     },
                     {
                       'Id' => '001',
                       'Mark1' => '82',
                       'Mark2' => '86',
                       'Mark3' => '98',
                       'Mark4' => '90'
                     },
                     {
                       'Address' => {
                         'DoorNo' => '69',
                         'Street' => 'BELL STREET'
                       },
                       'Country' => 'UK',
                       'Id' => '002',
                       'Name' => 'YYYYYYY'
                     },
                     {
                       'Id' => '002',
                       'Mark1' => '88',
                       'Mark2' => '69',
                       'Mark3' => '89',
                       'Mark4' => '39'
                     }
                   ]

# LICENSE

Copyright (C) 2016 Venkatesan Narayanan

# AUTHORS

Venkatesan Narayanan, <venkatesanmusiri@gmail.com>
