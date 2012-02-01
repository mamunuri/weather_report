require 'rho/rhocontroller'
require 'helpers/browser_helper'

class WeatherController < Rho::RhoController
  include BrowserHelper
  require 'json'
  require 'rexml/document'

  # GET /Weather
  def index
    @weathers = Weather.find(:all)
    render :back => '/app'
  end

  # GET /Weather/{1}
  def show
    @weather = Weather.find(@params['id'])
    if @weather
      render :action => :show, :back => url_for(:action => :index)
    else
      redirect :action => :index
    end
  end

  # GET /Weather/new
  def new
    @weather = Weather.new
    render :action => :new, :back => url_for(:action => :index)
  end

  # GET /Weather/{1}/edit
  def edit
    @weather = Weather.find(@params['id'])
    if @weather
      render :action => :edit, :back => url_for(:action => :index)
    else
      redirect :action => :index
    end
  end

  # POST /Weather/create
  def create
    @weather = Weather.create(@params['weather'])
    redirect :action => :index
  end

  # POST /Weather/{1}/update
  def update
    @weather = Weather.find(@params['id'])
    @weather.update_attributes(@params['weather']) if @weather
    redirect :action => :index
  end

  # POST /Weather/{1}/delete
  def delete
    @weather = Weather.find(@params['id'])
    @weather.destroy if @weather
    redirect :action => :index  
  end
 
  def weather_report
   render :action => :weather_report, :back => '/app'
  end 

  def show_report
    @city = @params['weather']['city']  
    Rho::AsyncHttp.get(
      :url => "http://www.google.com/ig/api?weather=#{@city}.xml",
      :headers => {"Content-Type"=>"application/xml"},
      :callback => url_for(:action => :httpget_callback) 
    )  
    @response["headers"]["Wait-Page"] = "true"
  end

  def show_weather_report
   @weather_report = REXML::Document.new(@@get_report)
    render :action => :show_weather_report, :back => :weather_report
  end

  def httpget_callback
    @@get_report = @params["body"]
    WebView.navigate( url_for(:action => :show_weather_report) )
  end

end
