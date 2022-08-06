# notes for lib draft

These note should not be merged in main (or...)

## First examples

### Basic example from caxlsx examples

```ruby
require 'xlsx_template'

template = XlsxTemplate.declare do
    worksheet do
        title 'Basic Worksheet'
        column(title: 'First', values: data[0]) 
        column(title: 'Second', values: data[1])
        column(title: 'Third', values: data[2]) 
    end
end

# Default behaviour if input_data is an array
data = [[1, 2, 3]]

template.serialize(input_data: data, output_file: 'basic_example.xlsx')
```

### Complex example from caxlsx examples

```ruby
require 'xlsx_template'

template = XlsxTemplate.declare do
    worksheet do
        title 'Basic Worksheet'
        column(title: 'First', values: data[0]) 
        column(title: 'Second', values: data[1])
        column(title: 'Third', values: data[2]) 
    end
end

# Default behaviour if input_data is an array
data = [[1, 2, 3]]

# To a file:
template.serialize(input_data: data, output_file: 'basic_example.xlsx')

# To a stream:
template.stream(input_data: data)

```

### Example taken from client need

```ruby
workbook 'spreadsheet_export' do
  # Set title. 
  # data is implicit here.
  title "My first export - My company - #{data.year} - #{data.start_at.strftime('%d-%m')} au ... - spreadsheet export"

  # ability do define an other file with styles definitions. Maybe use an other extention (xlsxstyles ?)
  styles_file 'export_comptable_styles.xlsx.rb'

  # example of a worksheet in partial
  worksheet(
    title: 'Elements variables de paie',
    partial: 'elements_variables_paie.xlsx.rb',
    locals: {evps: export_comptable.evp} # Référence used to the data used by this worksheet. Each level in the DS has its own "data"
  )

  # Worksheet defined in situ
  worksheet do
    title 'Absences'
    data data.rests
    content :absences
  end

  # Technically a worksheet is already a table. 
  # We may need to include multiple tables (one after the other or side by side)
  table name: :absences do
    header_style :bold          # définis plus tard dans le fichier style
    content do # Possibilité de passer une variable de bloc qui prend chaque ligne. Ex |rest|
      # Possibilité d'utiliser des variables. Par ex :
      # rest_presenter = Export:: ... ::RestPresenter.new(data)

      column(title: 'Nom', values: data.lastname) # possibilité de mettre du i18n dans le title
      column(title: 'Prénom', values: data.firstname)
      column(title: 'Matricule', values: data.todo) # TODO
      column(title: 'Type', values: data.rest_type) # TODO
      column(title: 'Rang', values: data.todo) # TODO
      column(title: 'Du', values: data.starts_at)
      column(title: 'Au', values: data.ends_at)
      column(title: 'Valeur', values: rest_presenter.duration_value)
      column(title: 'Unité', values: rest_presenter.duration_unit)
    end
  end
end

```
